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
```
