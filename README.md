## 📘 Git Commands

# Initialize a new Git repository
git init

# Add all files to the staging area
git add .

# Commit the staged files with a message
git commit -m "your message"

# Rename current branch to 'main' (forcefully)
git branch -M main

# Add a remote repository with the name 'origin'
git remote add origin https://github.com/your-username/your-repo.git

# Push current branch to origin and set upstream tracking
git push -u origin main

# Change the remote repository URL
git remote set-url origin https://github.com/your-username/new-repo.git

# Check the current remote URLs (fetch & push)
git remote -v

# Pull the latest changes from remote using rebase (cleaner history)
git pull --rebase

# Optional: remove existing remote (if needed)
git remote remove origin


# Check current branch and status
git status

# View current branches
git branch

# View commit history
git log
