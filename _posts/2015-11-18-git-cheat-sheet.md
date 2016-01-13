---
layout: post
title:  Git Cheat Sheet and Workflow
comments: True
---

I'm fairly new to using Git, so I'm constantly Googling how to do things.  This post is more about documenting the steps for myself, but I hope others are able to benefit from this as well.

## New Feature Workflow

When adding a new feature, or making any non-trivial change, do your work in a new branch!

``` bash
# Create new branch from master
git checkout -b new_feature master
```

Do some work, add some files and remember to commit often!

```bash
git add <file>
git commit -m "Adding new stuff that will do something cool"
```

While working on your new feature, it's a good practice to run a [rebase](https://help.github.com/articles/about-git-rebase/) and pull in any changes that have been made to the master branch.  This allows you to take care of merge conflicts sooner rather than waiting to fix them when you try to merge the branch.

```bash
#Rebase branch
git rebase -i master
```

When your new feature is ready to be shared with others, push it to Github so others can contribute.

```bash
##Push Branch to Github and issue pull request
git push -u origin new_feature
```

Now you can issue a [pull request](https://help.github.com/articles/using-pull-requests/) from Github.  It's always good to have peer review.  When your request has been approved you can merge the branch to Master (or another branch).  If you're done with the feature, you can delete your branch from the Github UI.  You can also delete a local branch this way:

```bash
# Delete new-feature branch
git branch -d new_feature
```

## Git Pull Alias

A nice shortcut to pull and rebase.  Run this before pushing your changes to avoid merge conflicts.

```
alias gitpull="git pull --rebase"
```

## Ignoring Files
I often forget to add a .gitignore file when creating a repository.  After I notice that there are files in the repo that I don't want or need I then need to add the .gitignore file, but this does not remove the exising file from the git repo.  To clean up these files you need to remove it from the index.  Run the following command, then commit your changes to stop tracking the files listed in .gitignore.

```
git rm --cached <file>
```

## Fixing Mistakes

Eventually you'll make a mistake and think you've lost all of your work.  The nice thing about Git is that there's almost always some way to get your *commited* work back.  If you're in that situation you can use `git reflog` to get a list of all commits.  To get back to a specific commit grab the SHA from reflog and use `git merge` to pull the changes back in.

## Links

* [Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/syncing)
* [Github Guides](https://guides.github.com/)
