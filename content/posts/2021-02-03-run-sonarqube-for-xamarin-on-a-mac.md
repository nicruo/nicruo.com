+++ 
date = 2021-02-03T19:00:00+00:00
title = "Run SonarQube for Xamarin on a Mac"
aliases = ['/2021/02/03/run-sonarqube-for-xamarin-on-a-mac', '/2021/02/03/run-sonarqube-for-xamarin-on-a-mac.html']
+++

I'm using SonarQube docker image

```
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest
```

After docker container is running go to [http://localhost:9000](http://localhost:9000):
```
login: admin 
password: admin
```
Configure your project as a C# project.

Get the project authentication token and save if for later.

Download the sonar SonarScanner for .NET Framework 4.6+ ([https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-msbuild/](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-msbuild/))

unzip it to a path of your choosing. And run the following command:

```
chmod +x <path of sonar scanner>/sonar-scanner-4.4.0.2170/bin/sonar-scanner              
```

To run the SonarScanner run the following commands:

```
mono <path of sonar scanner>/SonarScanner.MSBuild.exe begin /k:"project-key" /d:sonar.login="myAuthenticationToken" 

msbuild <path to solution.sln> /t:Rebuild

mono mono <path of sonar scanner>/SonarScanner.MSBuild.exe end /d:sonar.login="myAuthenticationToken"
```

Go back to your SonarQube website ([http://localhost:9000](http://localhost:9000) ) and check your project code analysis.