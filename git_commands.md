# Git global setup
git config --global user.name "David Fishman"
git config --global user.email "david.fishman@dfo-mpo.gc.ca"

# Create a new repository
git clone https://gccode.ssc-spc.gc.ca/David.Fishman/test.git
cd test
touch README.md
git add README.md
git commit -m "add README"
git push -u origin [branchname]

# Existing folder
cd existing_folder
git init
git remote add origin [git repo url]
git add .
git commit -m "Initial commit"
git push -u origin [branchname]

# Existing Git repository
cd existing_repo
git remote rename origin old-origin
git remote add origin https://gccode.ssc-spc.gc.ca/David.Fishman/test.git
git push -u origin --all
git push -u origin --tags

# List Remote repositories:
git remote -v

# Change URL of remote repo:
git remote set-url origin [new URL]

# Merge unrelated histories:
git pull origin [branchname] --allow-unrelated-histories

# Establish upstream / downstream:
git branch --set-upstream-to=origin/[branchname] [branchname]


# Check out which branch you are on:
git branch
