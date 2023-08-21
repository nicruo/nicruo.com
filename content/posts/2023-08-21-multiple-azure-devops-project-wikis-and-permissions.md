+++ 
date = 2023-08-21T14:26:10+01:00
title = "Multiple Azure DevOps project Wikis and Permissions"
+++

When you create a new Azure DevOps project, you have the option to create a project wiki or a wiki from a git repo within the current project.

![Create a project wiki or a wiki from a git repo](/img/2023-08-21-multiple-azure-devops-project-wikis-and-permissions/1.png)


For each project, you can have [one project wiki and many wikis from repos](https://learn.microsoft.com/en-us/azure/devops/project/wiki/provisioned-vs-published-wiki?view=azure-devops).

![Multiple wikis](/img/2023-08-21-multiple-azure-devops-project-wikis-and-permissions/2.png)

If you need to control permissions for your wikis, there are two places you need to look:

## Project wiki

For project wiki settings, select the wiki and choose More actions > Wiki security.

![More actions . . .](/img/2023-08-21-multiple-azure-devops-project-wikis-and-permissions/3.png)

![Wiki security](/img/2023-08-21-multiple-azure-devops-project-wikis-and-permissions/4.png)


## Wikis from repos

Wikis from repos are powered by an Azure Repo (even the project wiki is, under the hood, but that’s outside the scope of this discussion). To control permissions for these wikis, go to Project Settings / Repos / Repositories / Select your wiki repo / Security.

![Wiki from repo security](/img/2023-08-21-multiple-azure-devops-project-wikis-and-permissions/5.png)

One use case for controlling wiki permissions is the ability to have multiple wikis with different visibility settings for different groups or teams. 

Now go create multiple wikis!