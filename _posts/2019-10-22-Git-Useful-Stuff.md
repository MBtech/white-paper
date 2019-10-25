---
layout: post
title: "Git Gems"
categories:
  - guide
tags:
  - guide
---
**Ignoring changes to a committed file**

Committing a file and then ignore further changes to it
```shell
git update-index --assume-unchanged <filename>
```
To start tracking the files again
```shell
git update-index --no-assume-unchanged <filename>
```

**Stashing with Git**

`git stash` takes uncommitted changes and saves them for later use and then reverts them from your working copy.

Changes that have been stash can be unstashed using `git stash pop`. This will remove the changes and applies them to the working copy

`git stash apply` can be used to apply changes but not popping it from the stash

You can see the list of stashes using `git stash list`

and pop specific stashes using `git stash pop <stash ID>` e.g. `git stash pop stash@{2}`  

A detail discussion about git stash is in this [blog post](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

**References:**
- https://stackoverflow.com/questions/3319479/can-i-git-commit-a-file-and-ignore-its-content-changes
