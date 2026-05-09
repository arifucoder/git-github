## 📘 Git Commands (with Bangla Guide)

```bash
---------------- 🟢 Initial Setup ----------------

# Init repo
git init
# 👉 একটি নতুন Git রিপোজিটরি শুরু করে

# Rename branch to main
git branch -M main
# 👉 বর্তমান Master ব্রাঞ্চকে জোরপূর্বক 'main' নাম দেয়

# Add remote
git remote add origin https://github.com/your-username/your-repo.git
# 👉 লোকাল রিপোজিটরির সাথে GitHub রিপোজিটরি যুক্ত করে

# Change remote URL
git remote set-url origin https://github.com/your-username/new-repo.git
# 👉 রিমোট রিপোজিটরির URL পরিবর্তন করে

# Show remotes
git remote -v
# 👉 বর্তমানে সেট থাকা রিমোটগুলোর URL দেখায় (fetch/push)

# Remove remote
git remote remove origin
# 👉 রিমোট রিপোজিটরি `origin` কে মুছে ফেলে


---------------- 📝 Staging & Committing ----------------

# Stage all files
git add .
# 👉 সব ফাইলকে Git এর স্টেজিং area-তে যোগ করে

# Commit changes
git commit -m "message"
# 👉 একটি মেসেজ সহ পরিবর্তন সংরক্ষণ করে

# Check status
git status
# 👉 ফাইলের বর্তমান অবস্থা দেখায় (staged/unstaged/untracked)

# Show commit history
git log
# 👉 কমিট হিস্টোরি দেখায় (তারিখ, ম্যাসেজসহ)


---------------- 🚀 Pushing & Pulling ----------------

# Push & set upstream
git push -u origin main
# 👉 main ব্রাঞ্চকে GitHub-এ পাঠায় এবং future push/pull সহজ করে

# Pull with rebase
git pull --rebase
# 👉 রিমোট থেকে কোড নিয়ে নিজের কাজগুলো রিবেজ করে বসায়


---------------- 🌿 Working with Branches ----------------

# List all local branches
git branch
# 👉 লোকাল ব্রাঞ্চগুলোর তালিকা দেখায়

# Create new branch
git branch arif
# 👉 নতুন 'arif' নামে একটি লোকাল ব্রাঞ্চ তৈরি করে

# Switch to a branch
git checkout arif
# 👉 'arif' ব্রাঞ্চে সোয়াইচ করে (ব্রাঞ্চে কাজ শুরু করতে)

# OR: Create and switch at once
git checkout -b arif
# 👉 একইসাথে নতুন ব্রাঞ্চ তৈরি ও সোয়াইচ করে

# Make changes and commit again
git add .
git commit -m "your message"
# 👉 পরিবর্তন স্টেজ করে কমিট করে

# Push branch to GitHub without setting upstream
git push origin arif
# 👉 'arif' ব্রাঞ্চ GitHub-এ পাঠায়, কিন্তু upstream লিংক করে না

# Switch back to main branch
git checkout main
# 👉 আবার 'main' ব্রাঞ্চে ফিরে আসে

---------------- 🔀 Merging Branches ----------------

# Switch to main branch
git checkout main
# 👉 মূল ব্রাঞ্চে ফিরে যাও যেখানে merge করতে চাও

# Merge another branch into main
git merge arif
# 👉 'arif' ব্রাঞ্চের কাজগুলো main-এ যুক্ত করো (fast-forward or merge commit)

# If conflict happens, resolve the conflicting files manually
# 👉 কনফ্লিক্ট হলে সংশ্লিষ্ট ফাইল খুলে ঠিক করে নিতে হবে

# After resolving conflict, stage the changes
git add .
# 👉 কনফ্লিক্ট ঠিক করার পর পরিবর্তনগুলো স্টেজ করো

# Then complete the merge by committing
git commit -m "resolved merge conflict between main and arif"
# 👉 ম্যানুয়ালি কমিট করে merge সম্পূর্ণ করো


---------------- 📦 Deleting Branches ----------------

# Delete a local branch
git branch -d arif
# 👉 কাজ শেষে 'arif' ব্রাঞ্চ লোকাল থেকে মুছে ফেলো (if fully merged)

# Force delete a branch (if not merged)
git branch -D arif
# 👉 জোরপূর্বক ব্রাঞ্চ মুছে ফেলে (সাবধান হয়ে ব্যবহার করো)

# Delete a remote branch
git push origin --delete arif
# 👉 GitHub থেকে 'arif' ব্রাঞ্চ মুছে ফেলো


---------------- 📬 Pull Request (PR) Guide ----------------

# After pushing a new branch (e.g., arif), visit the following GitHub URL:
https://github.com/your-username/your-repo/pull/new/arif
# 👉 নতুন ব্রাঞ্চ GitHub-এ push করার পর ঐ ব্রাঞ্চ থেকে PR তৈরি করো

# Fill in title, description, and click "Create Pull Request"
# 👉 PR এর টাইটেল, বিবরণ দিয়ে তৈরি করো

# After review, the PR can be merged into main via GitHub interface
# 👉 রিভিউ শেষে GitHub থেকেই merge করা যায়


---------------- Last commit remove ----------------
# step 1: 
git reset --hard HEAD~1

# step 2:
git push origin main --force


```


```bash
---------------- repo commit author change ----------------
# STEP 1: old backup refs delete করো

rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now


# STEP 2: এবার full rewrite করো

FILTER_BRANCH_SQUELCH_WARNING=1 git filter-branch -f --env-filter '

export GIT_AUTHOR_NAME="arifuddincoder"
export GIT_AUTHOR_EMAIL="arifuddincoder@gmail.com"
export GIT_COMMITTER_NAME="arifuddincoder"
export GIT_COMMITTER_EMAIL="arifuddincoder@gmail.com"

' --tag-name-filter cat -- --all

# STEP 3: force push

git push origin --force --all



---------------- repo commit author change ----------------
# github logout
gh auth logout

# github login status check
gh auth status

# github login
gh auth login

# check করো তুমি কোন account use করছো:
git config user.name
git config user.email

# যদি wrong হয়, change করো:
git config --global user.name "arifuddincoder"
git config --global user.email "arifuddincoder@gmail.com"

```