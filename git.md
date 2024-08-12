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



## STAGE

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


## COMMIT

### git commit -m
```sh
$ git commit -m "[commit-message]"
```
> Commit your stagedc ontent as a new commit snapshot.


## PULL & PUSH

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
> Synchronizey the local repository with the remote repository on GitHub.

