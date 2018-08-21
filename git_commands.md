# Git Cheatsheet

## Git global setup
```
git config --global user.name "[first last]"
git config --global user.email "[email]"
```

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
