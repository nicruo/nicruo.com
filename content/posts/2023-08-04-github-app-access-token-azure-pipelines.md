+++ 
date = 2023-08-04T18:49:36+01:00
title = "Get GitHub App access token on Azure Pipelines"
slug = ""
+++

If you need to call some GitHub API endpoints from an Azure Pipeline, you may face the need to create a personal access token. In most situations, I would advise finding an alternative to PATs. Today we are going to see how we can do that with GitHub Apps.

You need to create a GitHub App with the desired permissions and install that GitHub App (Installing your own GitHub App - GitHub Docs).

After that, we need the App ID and the Private Key pem file that you can find in your GitHub App settings.

Now in Azure DevOps.

Go to Azure Pipelines > Libraries and create a Variable Group with the App ID key:

![Create Varible Group](/img/2023-08-04-github-app-access-token-azure-pipelines/create-vg.png)

And after, create a new secure file with the private key .pem file:

![Create Secure File](/img/2023-08-04-github-app-access-token-azure-pipelines/create-sf.png)

Now, let’s create a YAML pipeline starting with the following code:

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: githubapp-demo-vg

steps:
- task: DownloadSecureFile@1
  name: pemFile
  inputs:
    secureFile: 'key.pem'

- script: echo $(pemFile.secureFilePath)
- script: echo $(App ID)
...
```

With the AppID and SecureFile path in place, now we can add the PowerShell task that is going to return our GitHub App access token:

```yaml
...

- powershell: |
    Install-Module -Name jwtPS -Scope CurrentUser -Force
    Import-Module jwtPS -Force
    
    $header = @{
        "alg" = "RS256"
        "typ" = "JWT"
    }

    $payload = @{
        "iat" = [int][double]::parse((Get-Date -Date $((Get-Date).addseconds(-60).ToUniversalTime()) -UFormat %s))
        "exp" = [int][double]::parse((Get-Date -Date $((Get-Date).addseconds(10 * 60).ToUniversalTime()) -UFormat %s))
        "iss" = $(App ID)
    }

    $encryption = [jwtTypes+encryption]::SHA256
    $algorithm = [jwtTypes+algorithm]::RSA
    $alg = [jwtTypes+cryptographyType]::new($algorithm, $encryption)
    $jwt = New-JWT -Payload $payload -Algorithm $alg -FilePath $(pemFile.secureFilePath)

    $TokenUrl = "https://api.github.com/app/installations"
    $TokenHeaders = @{
        "Accept" = "application/vnd.github.machine-man-preview+json"
        "Authorization" = "Bearer $jwt"
    }

    try {
        $TokenResponse = Invoke-RestMethod -Uri $TokenUrl -Headers $TokenHeaders -Method Get
    } catch {
        Write-Error "Failed to get token response: $($_.Exception.Message)"
    }

    $InstallationId = $TokenResponse[0].id

    $TokenUrl = "https://api.github.com/app/installations/$InstallationId/access_tokens"
    $TokenHeaders = @{
        "Accept" = "application/vnd.github.machine-man-preview+json"
        "Authorization" = "Bearer $jwt"
    }

    $TokenResponse = Invoke-RestMethod -Uri $TokenUrl -Headers $TokenHeaders -Method Post

    $gh_accesstoken = $TokenResponse.token

    Write-Output $gh_accesstoken
...
```

Let’s run our pipeline and see that we got the access token returned:

![GitHub App access token returned](/img/2023-08-04-github-app-access-token-azure-pipelines/githubappaccesstoken.png)

Now it’s a matter of calling another request to the GitHub API to test if our token is working:

```yaml
...
    $TestUrl = "https://api.github.com/orgs/[your_org]/repos"
    $TokenHeaders = @{
        "Accept" = "application/vnd.github.machine-man-preview+json"
        "Authorization" = "Bearer $gh_accesstoken"
    }

    $testResponse = Invoke-RestMethod -Uri $TestUrl -Headers $TokenHeaders -Method Get

    $testResponse | Format-Table
```

![GitHub API response](/img/2023-08-04-github-app-access-token-azure-pipelines/githubapicall.png)

Here is the complete yaml file:
    
```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: githubapp-demo-vg

steps:
- task: DownloadSecureFile@1
  name: pemFile
  inputs:
    secureFile: 'key.pem'

- script: echo $(pemFile.secureFilePath)
- script: echo $(App ID)

- powershell: |
    Install-Module -Name jwtPS -Scope CurrentUser -Force
    Import-Module jwtPS -Force
    
    $header = @{
        "alg" = "RS256"
        "typ" = "JWT"
    }

    $payload = @{
        "iat" = [int][double]::parse((Get-Date -Date $((Get-Date).addseconds(-60).ToUniversalTime()) -UFormat %s))
        "exp" = [int][double]::parse((Get-Date -Date $((Get-Date).addseconds(10 * 60).ToUniversalTime()) -UFormat %s))
        "iss" = $(App ID)
    }

    $encryption = [jwtTypes+encryption]::SHA256
    $algorithm = [jwtTypes+algorithm]::RSA
    $alg = [jwtTypes+cryptographyType]::new($algorithm, $encryption)
    $jwt = New-JWT -Payload $payload -Algorithm $alg -FilePath $(pemFile.secureFilePath)

    $TokenUrl = "https://api.github.com/app/installations"
    $TokenHeaders = @{
        "Accept" = "application/vnd.github.machine-man-preview+json"
        "Authorization" = "Bearer $jwt"
    }

    try {
        $TokenResponse = Invoke-RestMethod -Uri $TokenUrl -Headers $TokenHeaders -Method Get
    } catch {
        Write-Error "Failed to get token response: $($_.Exception.Message)"
    }

    $InstallationId = $TokenResponse[0].id

    $TokenUrl = "https://api.github.com/app/installations/$InstallationId/access_tokens"
    $TokenHeaders = @{
        "Accept" = "application/vnd.github.machine-man-preview+json"
        "Authorization" = "Bearer $jwt"
    }

    $TokenResponse = Invoke-RestMethod -Uri $TokenUrl -Headers $TokenHeaders -Method Post

    $gh_accesstoken = $TokenResponse.token

    Write-Output $gh_accesstoken

    $TestUrl = "https://api.github.com/orgs/[your_org]/repos"
    $TokenHeaders = @{
        "Accept" = "application/vnd.github.machine-man-preview+json"
        "Authorization" = "Bearer $gh_accesstoken"
    }

    $testResponse = Invoke-RestMethod -Uri $TestUrl -Headers $TokenHeaders -Method Get

    $testResponse | Format-Table
```    

Using a GitHub App instead of a personal access token can be a more secure and efficient way to call GitHub API endpoints from an Azure Pipeline. By following the steps outlined above, you can easily set up a GitHub App and use it to obtain an access token for your API requests. This approach provides greater control over permissions and access, making it a preferred alternative to using PATs.