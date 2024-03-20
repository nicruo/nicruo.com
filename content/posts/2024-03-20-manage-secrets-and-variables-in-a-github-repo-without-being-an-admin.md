+++ 
date = 2024-03-20T14:35:00+00:00
title = "Manage Secrets and Variables in a GitHub Repo Without Being an Admin"
+++

Why: In a GitHub Organization, it is advised to reduce as much the number of repository administrators, to make governance easier. But there are some functionalities that we need that are only present (visually) for administrators.

In GitHub if you need to manage secrets or variables in a repository or environment, you can do it using the Settings page:

![Secrets and Variables Repository Settings](/img/2024-03-20-manage-secrets-and-variables-in-a-github-repo-without-being-an-admin/1.png)

The thing is, you need to have admin rights to the repository, to access the settings page.

Did you know that if you have write permissions, you can also manage these secrets and variables? Yes you can! But not with the GitHub UI (bummer, I know). You can use [GitHub API](https://docs.github.com/en/rest/actions/variables) or even better, you can with **GitHub CLI**.

I not going to explain how to install GitHub CLI (you can find it [here](https://cli.github.com)) , but will show you examples of using it to set, list and delete secrets and variables in repositories and environments.

## Manage secrets and variables in repositories

Set secret in repository:

```bash
gh secret set MYREPOSECRET --repo <owner/repo>
```

Set variable in repository:

```bash
gh variable set MYREPOVAR --repo <owner/repo>
```

List secrets in repository:

```bash
gh secret list --repo <owner/repo>
```

List variables in repository:

```bash
gh variable list --repo <owner/repo>
```

Delete secret in repository:

```bash
gh secret delete MYREPOSECRET --repo <owner/repo>
```

Delete variable in repository:

```bash
gh variable delete MYREPOVAR --repo <owner/repo>
```

## Manage secrets and variables in environments

Set secret in environment:

```bash
gh secret set MYENVIRONMENTSECRET --env MYENVIRONMENT --repo <owner/repo>
```

Set variable in environment:

```bash
gh variable set MYENVIRONMENTVAR --env MYENVIRONMENT --repo <owner/repo>
```

List secrets in environment:

```bash
gh secret list --env MYENVIRONMENT --repo <owner/repo>
```

List variables in environment:

```bash
gh variable list --env MYENVIRONMENT --repo <owner/repo>
```

Delete secret in environment:

```bash
gh secret delete MYREPOSECRET --env MYENVIRONMENT --repo <owner/repo>
```

Delete variable in environment:

```bash
gh variable delete MYENVIRONMENTSECRET --env MYENVIRONMENT --repo <owner/repo>
```

You can learn more about GitHub CLI commands at [GitHub CLI manual](https://cli.github.com/manual/).

While this is not as user-friendly as a Web UI, most developers nowadays are familiar with and use CLI tools daily.

Hope this enables more agility while using GitHub.
