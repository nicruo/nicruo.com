+++ 
date = 2024-01-23T10:26:00+00:00
title = "Prevent GitHub repository rename using rulesets"
+++

In a GitHub organization, you may find yourself in need of repository admins. While this can be challenging regarding governance, one thing that we usually don't want is for our repositories to be renamed. Today, I will show you how to prevent this using rulesets.

Rulesets is a GitHub functionality that protects our branches and tags, regarding who and what is allowed in our repositories. [You can learn more about rulesets in the GitHub Docs](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets).

![List of organization rulesets](/img/2024-01-23-prevent-github-repository-rename-using-rulesets/1.png)


To create a new ruleset, go to Organization / Settings / Rulesets / New Ruleset / New branch ruleset.

![New branch ruleset](/img/2024-01-23-prevent-github-repository-rename-using-rulesets/2.png)

We can manage who can bypass this ruleset based on Roles, Teams or GitHub Apps.

![Add bypass based on Roles, Teams or GitHub Apps](/img/2024-01-23-prevent-github-repository-rename-using-rulesets/3.png)

There is an option while creating a new branch ruleset that can prevent the renaming of all repositories or just the ones based on an [fnmatch syntax](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/creating-rulesets-for-a-repository#using-fnmatch-syntax).

![Dynamic list by name / name: * / Prevent renaming of target repositories](/img/2024-01-23-prevent-github-repository-rename-using-rulesets/4.png)

This option will prevent the renaming of every repository.

I have removed every other option on Branch protections in this ruleset.

![Remove every other option](/img/2024-01-23-prevent-github-repository-rename-using-rulesets/5.png)

With this ruleset enabled, if I try to rename the repository without a role to bypass, I see the error message "Name can't be changed on a repository protected by a ruleset".

![Ruleset enforced](/img/2024-01-23-prevent-github-repository-rename-using-rulesets/6.png)