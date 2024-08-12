# GIT Cheatsheet

## INITIALIZATION

### git init
```sh
$ git init
```
> Initialize an existing directory as a Git repository.

### git clone []
```sh
$ git clone [repository_url]
```
> Clone (download) a repository that already exists on GitHub, including all of the files, branches, and commits.

### git remote add []
```sh
$ git remote add origin [url]
```
> Link the local repository to an empty GitHub repository, after using the ``git init`` command.


## SETUP CREDENTIALS

### git config --global user.name
```sh
$ git config --global user.name "[firstname lastname]"
```
> Set a Username.

### git config --global user.email
```sh
$ git config --global user.email "[email]"
```
> Set an Email address.


## STAGE & COMMIT

### git status
```sh
$ git status
```
> Show modified files in the working directory, staged for the next commit.

### git add
```sh
$ git add [file-name]
```
> Add (stage) a file for the next commit.

### git add .
```sh
$ git add .
```
> Add all changed files to the staging area.

### git commit -m
```sh
$ git commit -m "[commit-message]"
```
> Commit your stagedc ontent as a new commit snapshot.

### git reset
```sh
$ git reset
```
> Reset staging area to match most recent commit, but leave the working directory unchanged. 

### git reset --hard
```sh
$ git reset --hard
```
> Reset staging area and the working directory to match most recent commit and overwrites all changes in the working directory. 

### git revert <commit>
```sh
$ git revert <commit>
```
> Create a new commit that undoes all of the changes made in, then apply it to the current branch.

### git stash
```sh
$ git stash
```
> Put current changes from your working directory into stash for later use.

### git stash pop
```sh
$ git stash pop
```
> Apply stored stash content into the working directory, and clear the stash.

### git stash drop
```sh
$ git stash drop
```
> Deletea specific stash from all your previous stashes.


## BRANCHES

### git branch
```sh
$ git branch
```
> Check the current branch.

### git branch [branch-name]
```sh
$ git branch [branch-name]
```
> Creates a new branch.

### git checkout -b [branch-name]
```sh
$ git checkout -b [branch-name]
```
> Creates a new branch and switch to that branch.

### git branch -d [branch-name]
```sh
$ git branch -d [branch-name]
```
> Deletes the specified branch.

### git pull origin <branch_name>
```sh
$ git pull origin <branch_name>
```
> Updates the current local working branch with all new commits from the corresponding remote branch on GitHub.
> ``git pull`` is a combination of git fetch and ``git merge``.

### git push origin <branch_name>
```sh
$ git push origin <branch_name>
```
> Uploads all local branch commits to GitHub.

### git fetch
```sh
$ git fetch
```
> Synchronize the local repository with the remote repository on GitHub.

### git merge
```sh
$ git merge <branch_name>
```
> Merge <branch> into the current branch.





