# Git Cheatsheet — বাংলায়

> দৈনন্দিন কাজে সবচেয়ে বেশি লাগে এমন Git command গুলোর সংক্ষিপ্ত রেফারেন্স।

---

## দ্রুত রেফারেন্স

```bash
# অবস্থা দেখা
git status                  # বর্তমান অবস্থা
git status -sb              # সংক্ষিপ্ত + branch

# পরিবর্তন stage করা
git add file.txt            # একটা file
git add .                   # সব
git restore --staged f.txt  # unstage করা

# Commit করা
git commit -m "message"     # commit
git commit -am "message"    # add + commit (tracked file)
git commit --amend          # শেষ commit ঠিক করা

# পুরনো/নির্দিষ্ট commit ঠিক করা
git rebase -i HEAD~3       # শেষ ৩টি commit দেখাবে; যেটা ঠিক করতে চান তার পাশে 'pick' বদলে 'edit' লিখে file save করে বের হন
git commit --amend         # commit-এর message বা content ঠিক করে save করুন
git rebase --continue      # rebase সম্পূর্ণ হবে; এবার git log-এ সব commit আবার দেখা যাবে

# পার্থক্য দেখা
git diff                    # unstaged পরিবর্তন
git diff --staged           # staged পরিবর্তন
git diff --stat             # শুধু সারমর্ম

# ইতিহাস দেখা
git log                     # পূর্ণ ইতিহাস
git log --oneline           # এক লাইনে
git log --oneline --graph --all --decorate   # সেরা combo

# Branch বদলানো
git switch main             # branch-এ যান
git switch -c feature       # নতুন branch বানিয়ে যান
git switch -                # আগেরটায় ফিরুন

# Branch মোছা
git branch -d feature       # নিরাপদ delete — merge না হলে মুছবে না
git branch -D feature       # জোর করে delete — merge না হলেও মুছে ফেলবে ⚠️

# Reset ও উদ্ধার
git reset --hard HEAD~1     # শেষ commit বাতিল, কাজসহ ⚠️ বিপজ্জনক
git reflog                  # HEAD-এর পুরো ইতিহাস — হারানো commit খুঁজুন
git reset --hard abc123     # reflog থেকে পাওয়া commit-এ ফিরে যান
git restore --staged index.html # index.html কে unstage করুন


# Revert — শেয়ার্ড/পাবলিক branch-এ safe undo
git revert asdb23                # নির্দিষ্ট commit-এর পরিবর্তন বাতিল করে নতুন commit যোগ করা
git revert HEAD                  # সর্বশেষ commit revert করা
git revert -m 1 <merge-commit>   # merge commit revert করতে হলে mainline নির্দিষ্ট করে দিতে হয়
git revert --no-commit asdb23    # revert করবে কিন্তু auto-commit করবে না, নিজে commit করতে হবে


# Stash — কাজ সাময়িকভাবে সরিয়ে রাখা
git stash                        # current changes stash করা (default message সহ) eta merge er poriborte, jate na haraiye jai switch korar time e
git stash push -m "message"      # নাম দিয়ে stash করা
git stash list                   # কতগুলো stash জমা আছে দেখা
git stash show                   # সর্বশেষ stash-এর changes দেখা
git stash pop                    # সর্বশেষ stash ফিরিয়ে আনা + list থেকে মুছে ফেলা
git stash apply                  # ফিরিয়ে আনা, কিন্তু list-এ থেকেই যাবে
git stash apply stash@{2}        # নির্দিষ্ট stash ফিরিয়ে আনা (index ধরে)
git stash drop stash@{1}         # নির্দিষ্ট stash মুছে ফেলা
git stash clear                  # সব stash মুছে ফেলা
```


# Git ও GitHub কমান্ড চিট শিট

## 🔗 Remote (GitHub-এর সাথে সংযোগ)

| কমান্ড | ব্যাখ্যা |
|---|---|
| `git remote add origin <repo-url>` | লোকাল রিপোজিটরির সাথে GitHub রিপোজিটরি যুক্ত করে (origin নামে) |
| `git remote -v` | কোন কোন remote যুক্ত আছে তা URL সহ দেখায় |
| `git remote set-url origin <new-url>` | origin-এর URL পরিবর্তন করে (ভুল URL দিলে বা repo বদলালে) |
| `git remote remove origin` | origin remote মুছে ফেলে |
| `git remote rename origin upstream` | remote-এর নাম origin থেকে upstream-এ বদলায় |

## 📦 Add ও Commit (পুশ করার আগের ধাপ)

| কমান্ড | ব্যাখ্যা |
|---|---|
| `git add <file-name>` | নির্দিষ্ট একটি ফাইল staging area-তে যোগ করে |
| `git add .` | সব পরিবর্তিত ফাইল একসাথে staging-এ যোগ করে |
| `git status` | কোন ফাইল add/commit হয়েছে বা হয়নি তা দেখায় |
| `git commit -m "message"` | staged ফাইলগুলো message সহ commit করে |
| `git commit -am "message"` | tracked ফাইলগুলো একসাথে add ও commit করে |

## ⬆️ Push (GitHub-এ আপলোড)

| কমান্ড | ব্যাখ্যা |
|---|---|
| `git push -u origin main` | প্রথমবার push করার সময় — main ব্রাঞ্চ GitHub-এ পাঠায় এবং tracking সেট করে |
| `git push` | tracking সেট থাকলে পরবর্তীতে শুধু এটুকুই যথেষ্ট |
| `git push origin <branch-name>` | নির্দিষ্ট ব্রাঞ্চ GitHub-এ push করে |
| `git push --force` | জোর করে push করে (⚠️ সাবধান — রিমোটের history মুছে যেতে পারে) |
| `git push origin --delete <branch>` | GitHub থেকে একটি ব্রাঞ্চ ডিলিট করে |

## ⬇️ Pull ও Fetch (GitHub থেকে ডাউনলোড)

| কমান্ড | ব্যাখ্যা |
|---|---|
| `git pull` | GitHub থেকে নতুন পরিবর্তন এনে লোকাল কোডের সাথে merge করে |
| `git pull origin main` | নির্দিষ্টভাবে origin-এর main ব্রাঞ্চ থেকে pull করে |
| `git fetch` | GitHub থেকে পরিবর্তন আনে কিন্তু merge করে না (আগে দেখে নিতে চাইলে) |
| `git pull --rebase` | merge commit না বানিয়ে নিজের commit-গুলো নতুন কোডের উপরে বসায় |

## 🌱 Clone ও শুরু করা

| কমান্ড | ব্যাখ্যা |
|---|---|
| `git clone <repo-url>` | GitHub থেকে পুরো রিপোজিটরি লোকালে কপি করে |
| `git init` | নতুন লোকাল Git রিপোজিটরি তৈরি করে |
| `git branch -M main` | বর্তমান ব্রাঞ্চের নাম main করে দেয় |

## 💡 সাধারণ workflow (নতুন প্রজেক্ট GitHub-এ তোলা)

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/username/repo.git
git push -u origin main
```
