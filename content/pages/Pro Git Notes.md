---
tags:
- Git
date: 2023-06-08
title: Pro Git Notes
categories:
lastMod: 2025-10-12
--- 
# Fundamentals
  
## Snapshots, not differences

Git thinks about its data as a stream of *snapshots*, not a base file with a stream of changes. Each commit creates a full snapshot.
  
## Git generally only Adds Data

Nearly all of operations in Git only *add* data to the database. It's hard to erase some data.
  
## Git Has Integrity

Everything in Git is checksumed (![using ]()SHA-1) and referred to by its hash.
  
## The Three States

Git is a mini file system. The tracked file can be in 3 states: *modified*, *staged*, and *committed*.
  
*Modified* means the file has changed but not commited to the database. Git only knows it changed. Using `git add` will make it *staged*.
  
*Staged* means you have marked a modified file in its **current** version to go into the next commit snapshot. In this time the current version of file is stored in *stage area*. Using `git commit` will make it *committed*.
  
*Committed* means the data has been stored in the database and have not been modified since last commit.
  
## Branch is a pointer

Branching is a "killer feature" of git. Git encourages workflows that branch and merge often. Then what is a branch?

When creating commits, you generally creates *blob* objects (content of a version of a file) and *commit* objects (containing a pointer to the snapshot, author's info and pointers to the parent(s) of this commit). A commit object have zero parents when it is init commit, one parent when normal committing, and multiple parents when merging from 2+ branches. Thus, the commits form a tree, each node points to its parent(s).

A branch is simply a lightweight movable pointer to one of the commits. The special pointer `HEAD` points to the branch you are currently on.

A branch is nothing but a simple file containing the 40 character hash of its pointing commit and a newline char. That is, a 41-byte file.
  
### Merge strategies

When you try to merge one commit with a commit that can be reached by following the first commit's history, Git simplifies things by moving the pointer forward because there're no divergent work to merge. This is called **fast-forward**.

When your commit has diverged from some older point, that is, the commit on the branch you're on isn't a direct ancestor of the branch you're merging in, Git does a three-way merge, using the two snapshots pointed to by the branch tips and the common ancestor of the two. Git will create a new snapshot that results from the three-way merge and automatically creates a commit (a merge commit with multiple parents) that points to it. This is called **recursive**. (You may have to resolve conflicts manually.)
  
# Quickref
  
## Short status

`git status -s` or `git status --short` gives a short version of `git status`.
  
## .gitignore
  
## View Staged and Unstaged Changes

To see the modified but not staged files, use `git diff`.

To see the staged but not committed files, use `git diff --staged` or `git diff --cached`. (`--cached` and `--staged` are synonyms)
  
## Get a more explicit reminder of changes when committting

Use `git commit -v` will puts the diff in the editor so you can see exactly what will be committed.
  
## Skipping the staging process

**This is not recommended since unwanted changed may be committed.**

Use `git commit -a`. Git will automatically stage every tracked file before committing. That is, skipping the `git add`.
  
## Removing files

`git rm` will remove the file from **both working tree and staging area.**

`git rm --cached` will remove the file only from staging area.
  
## View History

For code review, use `git log -p -2`  to see the last 2 commit history, with patch output (diff). 

For an more abbreviated version, use `--stat`.

`--pretty=` can receive a lot of options. such as `oneline`, `short`, `full`, `fuller` or even `format:"<format description>"`.

`git log --graph` shows an ASCII graph of branch and merging history.

Use `git log --since=2.weeks` or `git log --until=2008-01-15` to limit output using dates.

Use `git log --grep` to show only commits that contains given string.
  
## Undoing things

This is the few areas in Git that do not add data. Be careful.
  
### Uncommit

When you commit too early and forget to add some files, or mess up the commit message, you want to redo the commit, use `git commit --amend`.

**Only amend commits that are still local and not been pushed somewhere. Amending previously pushed commits and force pushing the branch will cause problems for your collaborators.**
  
### Unstage

Use `git reset HEAD <file>` or `git restore --staged <file>`(recommended, introduced in git 2.23.0) to unstage a file.
  
### Unmodify

**This is a dangerous command. Don't use it unless you absolutely know that you don't want the unsaved changes.**

Use `git checkout -- <file>`  or `git restore <file>` (recommended, introduced in git 2.23.0) to discard changes in working directory (and replace it with the last staged/committed version).
  
## Working with remotes

`git remote -v` shows the URLs and shortnames of remotes.

`git remote add <shortname> <URL>` to add remote.

`git fetch`  goes out to the remote and downloads all the data from that remote that you don't have yet, but it doesn't automatically merges it with any of your work or modify what you are currently working on. You have to merge it manually.

If your current branch is set to track a remote branch, `git pull` will fetch and automatically merge the remote branch into your current branch.

`git remote show` will tell you which branch is automatically pushed to, and which remote branches you don't yet have, which branches you have have been removed from the server.
  
## Tagging

Git have a tag feature. People often use tags to mark release points.

Use `git tag` (or with optional `-l` or `--list`) to list the existing tags (in alphabetical order).

Use `git tag -l "v1.8.5*"` to list the 1.8.5 series.

Use `git tag -a <tag name> -m <tag message>` to create an annotated tag. The tag data along with the commit can be shown using `git show <tag name>`.

Use `git tag <tag name>` to create a light-weight tag.

Use `git tag -a <tag name> <commit hash>` to tag a former commit.

By default, `git push` doesn't transfer tags to remotes. Use `git push origin <tag name>` to push a tag.  `git push --tags` to push all tags. `git push --follow-tags` to push only annotated tags.

Use `git tag -d` to delete tags.

Use `git checkout <tag name>` to check out tag, and here you will enter "detached HEAD" state.
  
## Alias

Use `git config --global alias.unstage "reset HEAD --"` will make the following commands equivalent:

```git
$ git unstage fileA
$ git reset HEAD -- fileA
```
  
## Branching

`git switch <branch>` to switch, `git switch -c <branch>` to create and switch.

When you switch branches in git, files in your working directory will change. Git will replace them as what they like in that branch. If Git cannot do it cleanly, it will not let you switch at all.

To show the last commit on each branch, use `git branch -v`.

To show the tracking branches and ahead/behind info, use `git branch -vv`. Notice this command does not reach out to the servers. It just tells you info since the last time you fetched.

`--merged` and `--no-merged` can be provided as param to filter branches. Do not delete no-merged branches.

Let a branch track some remote branch: `git branch -u <remote name>/<branch name>` or full version: `git branch --set-upstream-to <remote name>/<branch name>`.

When you have a tracking branch set up, you can reference its upstream branch with `@{upstream}` or `@{u}` shorthand. e.g. `git merge @{u}`.

Delete a remote branch using `git push <remote name> --delete <branch name>`.

Rename: `git branch --move <original branch name> <new branch name>`.
  
### See diverge history

`git log --oneline --decorate --graph --all` will print the history of commits, show where your branch pointers are and how the history has diverged.
  
## Share your Work

`git push <remote name> <local branch name>:<remote branch name>` .
  
## Rebasing

---

To be continued..

19% ███░░░░░░░░░░░░░░░░░ 100%
 