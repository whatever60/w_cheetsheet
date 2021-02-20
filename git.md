# Git cheetsheet

## Git basic

### Commit

`git commit -m "Completed something"`

### Add: add files to staing area

`git add` / `git add -A` / `git add -all`

Stage all your changes, including modified, deleted and untracked files in the entire working tree.

"Entire working tree" means even if you run this command in a subdirectory, all changes in all directories will be staged.

But you can also specify a directory and only add changes within that directory, such as `git add -A my_dir/`

Note: `git add .` is the same as `git add -A .`, since `-A` is the default of `git add`, so only when you are in the top directory, `git add .` is the same as `git add -A`.

Note: do not use `git add *`. `*` is handled by your shell, not git, so it might bring unexpected results. For example, dotfiles will not be included, deleted files in the current directory will not be included, but deleted files in subdirectories will be included.

`git add --no-all` / `git add --ignore-removal`

Stage all changes, except for deleted files.

`git add -u` / `git add --update`

Stage all changes, except for untracked files

### Add to staging area

`git add -A`

`git status`

`git diff`

`git diff <hash1> <hash2>`

`git log`

`git log --stat`

`git branch`

### `reset`: remove files from staging area

`git reset`

unstage all the changes

### `branch`

`git branch -a`

List all branches, both local and remote

### `remote`

`git remote -v` 

## Fixing mistakes and undoing bad commits

Ref:

[Git Tutorial: Fixing Common Mistakes and Undoing Bad Commits](https://www.youtube.com/watch?v=FdZecVxzJbk) by Corey Schafer

### Back to the way that things were at the last commit

`git checkout calc.py`

### Modify commit message

`git commit --amend -m "the right message"`

Note: --amend flag will change git history, so only do this before you push the changes to other people

### Make the new commit a part of the last commit

`git commit --amend`

Note: git history will be changed

### Move a commit of one branch to another branch

This can be done in a few steps.

Switch to the target branch

`git checkout target-branch`

Create a new commit based off of our original

`git cherry-pick <hash of the commit of source branch>`

Switch back and get rid of the old commit and changes of the source branch

`git checkout source-branch`

`git reset --soft <hash of the commit we want to be the last commit>`

or

`git reset <hash>`

or

`git reset --hard <hash>`

Note:

--soft flag resets us back to the commit we specified but still keeps the changes, both in working directory and staging area.

--mixed flag (default) keeps changes in working directory, but not in staging area

--hard flag reverts all tracked files to the state they were in at the hash that we specify. (get rid of all changes, but untracked files will be left alone)

Get rid of untracked files

`git clean -df`

Note: -d flag gets rid of any untracked directories and -f flag any untracked files

### What if you deleted changes that you actually need

Show commands in the order when you last referenced them

`git reflog`

Note: reflog will be garbage-collected after certain time

Grab the hash before the command you want to return

`git chechout <hash>`

Note: you are now in a "detached head state", which will be trashed at some point in the future, so you need to make a branch from it to save it.

`git branch branch-name`

### Create new commits to revert the effect of earlier commits (won't rewrite history)

`git revert <hash>`
