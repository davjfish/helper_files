# Git Cheatsheet

## Git global setup
```
git config --global user.name "[first last]"
git config --global user.email "[email]"
```
- store credentials in system `git config credential.helper store` you will run this and then the next time you enter credentials it will be stored for a while.
- use `git config --unset credential.helper` to unstore. both above and this are for local repos
## Create a new repository
```
git clone [repo URL]
cd test
touch README.md
git add README.md
git commit -m "add README"
git push -u origin [branchname]
```

## From an Existing folder
```
cd [existing_folder]
git init
git remote add origin [remote URL]
git add .
git commit -m "Initial commit"
git push -u origin [branchname]
```

## Existing Git repository
```
cd [existing_repo]
git remote rename origin old-origin
git remote add origin [remote URL]
git push -u origin --all
git push -u origin --tags
```

## List Remote repositories:
```
git remote -v
```

## Change URL of remote repo:
```
git remote set-url origin [new URL]
```

## Merge unrelated histories:
```
git pull origin [branchname] --allow-unrelated-histories
```

## Establish upstream / downstream:
```
git branch --set-upstream-to=origin/[branchname] [branchname]
```


## Check out which branch you are on:
```
git branch
```

## Untrack a file that was just added to .gitignore
```
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

## check to see which files were modified during a specific commit
```
git diff-tree --no-commit-id --name-only -r <commit-ish>
```

## Submodules
- create a unique repo for the Submodules
- go to the root dir of the project where you want to import the submodule and type" `git submodule add [URL-to-repo] [optional path to dir]`
- then add / commit changes, both within the main repo and the submodule dir
- after adding a submodule in a local repo, when you try to pull the changes to another local repo, it will only import the root submodule dir. In order to configure, use the following `git submodule update --init --recursive`

## undo a "git add"
- `git reset` to reset entire repository
- `git reset [filename]` to unadd a file

## reset a file to the last commit
- 'git checkout [filename]'

## To revert to a previous commit, ignoring any changes:
`git reset --hard HEAD`

## To force overwrite of local file with what's on origin/master
- If you want to overwrite only one file:
```
git fetch origin
git checkout origin/master -- <filepath>
```
- or more generally:
`git checkout [BRANCH] -- <filepath>`

- If you want to overwrite all changed files:
```
git fetch origin
git reset --hard origin/master
```

## Branches
- `git branch` show which branch you are on and what 
- `git branch [branchname]` create a branch
- `git checkout [branchname]` switch into a branch
- from the master branch... `git merge [branchname]` to merge the changes
- `git push origin [branchname]` push up a branch to the remote repo
- `git log --graph --decorate --oneline` visualize git history
- `git branch -d branch_name` delete a branch locally
- `git push --delete <remote_name> <branch_name>` delete a branch on the remote
- `git checkout -b [branch] [remote]/[branch]` import a branch from the remote repo
- OR `git fetch --all` followed by `git checkout [branch_name]` 

### Comparing Branches
- `git diff --name-status master` compare current branch to master;
- `git diff master..otherbranch > myDiffFile.diff` compare two branches and pipe to file; will only look at committed changes

## do a git pull and overwrite any files in the local repo
```
git fetch --all
git reset --hard origin/master
```

### interact with another local repo
- clone from a local repo:
```
git clone /path/to/local/repo
```
- pull from a local repo
```
git remote add repo_b /path/to/repository
git pull repo_b master
```

### Delete local reference to deleted branches and display verbose
```
git fetch --prune -v
```

### See the commit version / hash 
`git rev-parse --short HEAD`
