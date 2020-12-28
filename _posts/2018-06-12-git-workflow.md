---
layout: post
title: Everything about Git
tags:
  - Linux
---

* TOC
{:toc}

## Git Commands
1. initialization: `git init`
2. add all files: `git add --all` (`git add .`)
3. remove staged files: `git reset <file>`
4. delete files from disk and stage the changes: `git rm <file>`
5. commit: `git commit -m "<message>"`
6. amend commit: merge staged change into last commit: `git commit --amend`
7. add remote repo: `git remote add <name> <url>.git`
8. push to remote: `git push <remote_name> <branch_name>`. Use `git push -u <emote_name> <branch_name>` for the first time to let Git remember what to do, so next time you only need to type `git push`.
9. create a new branch from a commit and switch to it: `git checkout -b <branch_name> [hash for commit]`
10. remove a branch: `git branch -D <branch_name>`
11. remove a remote branch: `git push origin --delete <branch_name>`
12. list all branch: `git branch`
13. list all commit in this branch: `git log` (use `q` to exit). Use `git log --pretty=oneline` to make each commit takes one line
14. revert to a previous commit: `git reset --hard <hash>`. To revert last n commits, use `git reset —hard HEAD~n`
15. switch to a branch: `git checkout <branch_name>`
16. merge branch A into branch B: `git merge A B` (create a new merge commit in branch B)
17. rebase: `git rebase A` will combine commit history of branch A into B, forming a linear history: `git rebase --interactive HEAD~<i>` where `<i>` is the number of commits you want to combine. It will open vim. Change all pick to stash except first line. Then edit second file for commit message.
18. cherry-pick: apply a specific commit to current branch.


## Standard Workflow and Best Practices

For each feature, a new branch should be created.
1. Pull the latest code on master from remote.
2. Create a new branch.
3. Commit regularly.
4. After feature is complete, squash commits with `git rebase` into a single commit.
5. Push the feature branch to remote and send pull request.
6. Rebase feature branch onto master and merge feature branch onto master.

### Best Practice to Merge to Mainline
1. Commit and clean in the working branch: `git commit`
2. Pull everything into mainline. Because we haven't made any changes to mainline, pull would be fast-forwarding. `git checkout mainline` and `git pull`.
3. Checkout the working branch, rebase changes in mainline to working branch (all changes on current branch will be on top of mainline’s change): `git checkout <change_branch>` and `git rebase mainline`.
4. Checkout mainline and merge working branch (fast-forwarding): `git checkout mainline` and `git merge <change>`.

Note: if all changes are already in mainline, use `git pull -r` to rebase local changes on top of remote changes.

### How to Write the Commit Message
```plain
Present-tense summary under 50 characters
<newline>
* More information about this commit (under 72 characters)
* More information about this commit (under 72 characters)
<newline>
Link to reference, bug ticket, etc.
```

### Git rebase

Rebase is another way (besides merge) to solve conflicts between branches. It takes the common ancestor of the two branches, save all changes on current branch and reset current branch to common ancestor, pull all changes from the other branch, and then apply all saved changes on top of it, forming a linear history.

There are six commands available while rebasing:

#### pick
`pick` simply means that the commit is included. Rearranging the order of the pick commands changes the order of the commits when the rebase is underway. If you choose not to include a commit, you should delete the entire line.

#### reword
The `reword` command is similar to pick, but after you use it, the rebase process will pause and give you a chance to alter the commit message. Any changes made by the commit are not affected.

#### edit
If you choose to `edit` a commit, you'll be given the chance to amend the commit, meaning that you can add or change the commit entirely. You can also make more commits before you continue the rebase. This allows you to split a large commit into smaller ones, or, remove erroneous changes made in a commit.

#### squash
This command lets you combine two or more commits into a single commit. A commit is squashed into the commit above it. Git gives you the chance to write a new commit message describing both changes.

#### fixup
This is similar to squash, but the commit to be merged has its message discarded. The commit is simply merged into the commit above it, and the earlier commit's message is used to describe both changes.

#### exec
This lets you run arbitrary shell commands against a commit.

#### References
* [Reference 1](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
* [Reference 2](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

### More on Git Log
`git log` supports many parameters. Most frequently used are:
* `--author`: only show this user
* `--name-only`: only show names of changed files
* `--online`: output one line per commit
* `--graph`: show commit tree
* `--reverse`: reverse the order of commits (earliest first)
* `--after`: only show commits after certain time
* `--before`: only show commits before certain time
* `-L`: show commits related to some lines in a file

## Unicode Support
In certain OS, Git may not render non-English characters in file names correctly. If this case occurs, run

```bash
git config --global core.quotePath false
```

If it still does not fix the problem on cacOS, run

```bash
git config --global core.precomposeunicode true
```

## Git Internal
Git is a stupid context checker. It is one of the most popular revision control system, i.e. software change and configuration management system. Traditional RCS is central, meaning repo is stored on a remote server, and all clients communicate with server on commit. The biggest difference for Git is that Git is distributed. Every clone is really full backup of all data. After initial clone, almost every operation is local. Thus, it is fast and scalable (local changes are invisible to others)

It provides two level of commands:
* Porcelain: user-friendly commands
* Plumbing: low-level commands

Git’s version database:
* Content addressable file system, a simple key-value data store
* Key: SHA-1 hash (distinct)
* Value: binary files:
    * Commits: actual git commits, a snapshot
    * Trees: directories, structure of file system
    * Blobs: content of files/data

* git init creates a git repo, with a .git folder. In .git folder, usually we can find a HEAD, objects folder, a ref folder with at least tags and heads subfolder in it.
    * The object folder contains all hashes, and it uses a trie to store all hashes (another reason is on some file system, a folder can contain a maximum number of files). Multiple files with same content can share the same hash file.
    * HEAD contains a reference to a commit in ref folder
    * index file contains file location
* After we use git add, it creates an index file that stores information about tracking files, and calculate and store hashes
* git commit creates a hash (the commit hash), it contains a tree hash that contains the file info (which points to the blob or other trees if it’s a folder), author, committer, and commit message
