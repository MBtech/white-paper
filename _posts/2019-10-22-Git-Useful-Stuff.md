---
layout: post
title: "Git Gems"
categories:
  - guide
tags:
  - guide
---
Here are a set of commands that I find useful when using Git

**Ignoring changes to a committed file**

Committing a file and then ignore further changes to it
```bash
git update-index --assume-unchanged <filename>
```
To start tracking the files again
```bash
git update-index --no-assume-unchanged <filename>
```

**Get log of git commits**
```bash
git log
```
**Delete a local commit**
```bash
git reset --hard HEAD~
```
This will reset the state of the repository. If the excess commits are available to others as well you might want to use `git revert` instead.

**Delete a remote commit**

First remove the commit from your local repo (see above)
```bash
git push origin +master
```

**Stashing with Git**

`git stash` takes uncommitted changes and saves them for later use and then reverts them from your working copy.

Changes that have been stash can be unstashed using `git stash pop`. This will remove the changes and applies them to the working copy

`git stash apply` can be used to apply changes but not popping it from the stash

You can see the list of stashes using `git stash list`

and pop specific stashes using `git stash pop <stash ID>` e.g. `git stash pop stash@{2}`  

A detail discussion about git stash is in this [blog post](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

**Git Submodules**
https://git-scm.com/book/en/v2/Git-Tools-Submodules

**References:**
- https://stackoverflow.com/questions/3319479/can-i-git-commit-a-file-and-ignore-its-content-changes
