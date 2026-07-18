# Git Details — বাংলায়

> দৈনন্দিন কাজে সবচেয়ে বেশি লাগে এমন Git command গুলোর সংক্ষিপ্ত রেফারেন্স।

---

## এক নজরে

| Command | কাজ কী |
|---|---|
| `git status` | এখন কী অবস্থায় আছে, তা দেখায় |
| `git add` | পরিবর্তনগুলো staging area-তে নেয় |
| `git commit` | Staged পরিবর্তন স্থায়ীভাবে সংরক্ষণ করে |
| `git diff` | ঠিক কোন লাইন বদলেছে তা দেখায় |
| `git log` | Commit-এর ইতিহাস দেখায় |
| `git log --oneline` | ইতিহাস সংক্ষেপে, এক লাইনে |
| `git switch` / `git checkout` | Branch বদলায় |

---

## Git-এর তিনটা জায়গা

কমান্ডগুলো বোঝার আগে এই তিনটা ধাপ মাথায় রাখুন — সব কমান্ড এই ধাপগুলোর মাঝেই কাজ করে।

```
Working Directory  →  Staging Area  →  Repository
   (আপনি যেখানে         (git add)        (git commit)
    edit করেন)
```

- **Working Directory** — আপনার আসল file, যেখানে আপনি লিখছেন
- **Staging Area** — যেসব পরিবর্তন পরের commit-এ যাবে, সেগুলোর অপেক্ষার ঘর
- **Repository** — commit হিসেবে স্থায়ীভাবে জমা হওয়া ইতিহাস

---

## `git status`

**কী করে:** এই মুহূর্তে repository-র অবস্থা দেখায় — কোন file বদলেছে, কোনটা staged হয়েছে, কোনটা এখনো Git-এর নজরেই আসেনি (untracked)। কোন branch-এ আছেন সেটাও বলে দেয়।

```bash
git status
```

**কাজের variation:**

```bash
git status -s          # short/সংক্ষিপ্ত ফরম্যাটে দেখায়
git status -sb         # সংক্ষিপ্ত + branch-এর নামসহ
```

**`-s` output পড়ার নিয়ম:**

| চিহ্ন | মানে |
|---|---|
| `??` | Untracked — Git এই file-টা এখনো চেনে না |
| `M` (বাম ঘরে) | Staged modification |
| `M` (ডান ঘরে) | Modified কিন্তু এখনো staged হয়নি |
| `A` | নতুন file যোগ করা হয়েছে (staged) |
| `D` | Deleted |

> 💡 কনফিউজড লাগলে সবার আগে `git status` চালান। এটা সাধারণত পরের করণীয়টাও suggest করে দেয়।

---

## `git add`

**কী করে:** Working directory-র পরিবর্তনগুলো **staging area**-তে নিয়ে যায়, অর্থাৎ "এই পরিবর্তনগুলো আমি পরের commit-এ রাখতে চাই" — এটা Git-কে জানায়। `git add` না করলে commit-এ কিছুই যাবে না।

```bash
git add file.txt           # নির্দিষ্ট একটা file
git add file1.js file2.js  # একাধিক file একসাথে
git add .                  # বর্তমান folder ও তার ভেতরের সব পরিবর্তন
git add -A                 # পুরো repo-র সব পরিবর্তন (delete সহ)
git add *.md               # নির্দিষ্ট pattern-এর সব file
```

**সবচেয়ে কাজের option:**

```bash
git add -p                 # একই file-এর কিছু অংশ নেবেন, কিছু নেবেন না
```

`-p` (patch mode) একেকটা পরিবর্তনের টুকরো দেখিয়ে জিজ্ঞেস করে — `y` (নেব), `n` (নেব না), `s` (আরও ছোট টুকরো করো), `q` (বেরিয়ে যাও)। বড় কাজকে ছোট ছোট পরিষ্কার commit-এ ভাগ করার জন্য দারুণ।

**ভুল করে add করে ফেললে (unstage):**

```bash
git restore --staged file.txt   # আধুনিক পদ্ধতি
git reset HEAD file.txt         # পুরোনো পদ্ধতি, একই কাজ করে
```

---

## `git commit`

**কী করে:** Staging area-তে থাকা পরিবর্তনগুলোর একটা স্থায়ী **snapshot** তৈরি করে ইতিহাসে জমা রাখে। প্রতিটা commit-এর একটা unique ID (hash), লেখকের নাম, সময় আর একটা message থাকে।

```bash
git commit                        # editor খুলে message লিখতে বলবে
git commit -m "Add login form"    # সরাসরি message দিয়ে commit
```

**খুব কাজের shortcut:**

```bash
git commit -am "Fix typo"   # tracked file গুলো add + commit একসাথে
                            # ⚠️ untracked (নতুন) file এতে যাবে না
```

**শেষ commit ঠিক করা:**

```bash
git commit --amend                    # শেষ commit-এর message বদলান
git commit --amend --no-edit          # ভুলে যাওয়া file যোগ করুন, message একই থাকবে
```

> ⚠️ `--amend` ইতিহাস বদলে দেয়। যে commit ইতিমধ্যে push করা হয়ে গেছে, সেটাতে amend করবেন না (যদি না আপনি একাই ওই branch ব্যবহার করেন)।

**ভালো commit message-এর নিয়ম:**

- বর্তমান কালে, আদেশের সুরে লিখুন: `Add search filter` — `Added` বা `Adds` নয়
- প্রথম লাইন ৫০ character-এর মধ্যে রাখুন
- বিস্তারিত লাগলে একটা খালি লাইন দিয়ে তারপর বডি লিখুন
- **কী** করেছেন তার চেয়ে **কেন** করেছেন সেটা বেশি জরুরি — কী করেছেন সেটা তো diff দেখলেই বোঝা যায়

---

## `git diff`

**কী করে:** ঠিক **কোন লাইনগুলো বদলেছে** তা লাইন ধরে ধরে দেখায়। `git status` বলে "কোন file বদলেছে", আর `git diff` বলে "সেই file-এ ঠিক কী বদলেছে"।

```bash
git diff                    # যা এখনো staged হয়নি (working dir vs staging)
git diff --staged           # যা staged হয়েছে কিন্তু commit হয়নি
git diff HEAD               # শেষ commit-এর তুলনায় সব পরিবর্তন
git diff file.txt           # শুধু একটা file-এর diff
```

**তুলনা করার আরও উপায়:**

```bash
git diff main feature       # দুই branch-এর মধ্যে পার্থক্য
git diff abc123 def456      # দুই commit-এর মধ্যে পার্থক্য
git diff HEAD~1 HEAD        # শেষ commit-টা ঠিক কী বদলেছিল
git diff --stat             # শুধু সারমর্ম — কোন file-এ কয় লাইন বদলেছে
```

**Diff পড়ার নিয়ম:**

| চিহ্ন | মানে |
|---|---|
| `-` (লাল) | এই লাইনটা মুছে গেছে |
| `+` (সবুজ) | এই লাইনটা যোগ হয়েছে |
| `@@ -3,7 +3,8 @@` | কোন লাইন নম্বর থেকে পরিবর্তন শুরু |

> 💡 **অভ্যাস করুন:** commit করার আগে সবসময় `git diff --staged` চালিয়ে দেখে নিন ঠিক কী commit হতে যাচ্ছে। অনেক ভুল এখানেই ধরা পড়ে।

---

## `git log`

**কী করে:** Commit-এর **ইতিহাস** দেখায় — সবচেয়ে নতুনটা সবার উপরে। প্রতিটা commit-এর hash, লেখক, তারিখ আর message দেখা যায়।

```bash
git log
```

চলার সময় লম্বা output হলে pager-এ আটকে যাবেন — **`q` চাপলে** বেরিয়ে আসবেন, তীর চিহ্ন দিয়ে scroll করবেন।

**Filter করার উপায়:**

```bash
git log -5                          # শেষ ৫টা commit
git log --author="Rahim"            # নির্দিষ্ট লেখকের commit
git log --since="2 weeks ago"       # গত ২ সপ্তাহের commit
git log file.txt                    # শুধু এই file-এর ইতিহাস
git log --grep="bug"                # message-এ "bug" আছে এমন commit
git log -p                          # প্রতিটা commit-এর সাথে পুরো diff
```

---

## `git log --oneline`

**কী করে:** একই ইতিহাস, কিন্তু প্রতিটা commit **এক লাইনে** — শুধু ছোট hash আর message। ইতিহাসের উপর দ্রুত চোখ বুলানোর জন্য সবচেয়ে ব্যবহৃত ফর্ম।

```bash
git log --oneline
```

Output দেখতে এরকম:

```
a1b2c3d Add login validation
e4f5g6h Fix navbar spacing
i7j8k9l Initial commit
```

**সবচেয়ে কাজের combo (branch structure সহ):**

```bash
git log --oneline --graph --all --decorate
```

- `--graph` — branch আর merge-এর গঠন ASCII ছবিতে দেখায়
- `--all` — শুধু বর্তমান branch নয়, সব branch দেখায়
- `--decorate` — branch আর tag-এর নাম দেখায়

**এটা alias বানিয়ে রাখুন — খুব কাজে দেবে:**

```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
```

এরপর শুধু `git lg` লিখলেই হবে।

---

## `git switch` / `git checkout`

**কী করে:** এক branch থেকে আরেক branch-এ যাওয়ার কমান্ড। `git switch` নতুন (Git 2.23+) এবং **শুধু branch বদলানোর জন্যই** বানানো, তাই এটাই বেশি নিরাপদ ও স্পষ্ট। `git checkout` পুরোনো, এটা branch বদলানো + file restore করা — দুটো কাজই করে, যেটা বিভ্রান্তিকর।

### `git switch` — আধুনিক ও সুপারিশকৃত

```bash
git switch main             # main branch-এ যান
git switch -c new-feature   # নতুন branch বানিয়ে সেখানে যান
git switch -                # আগের branch-এ ফিরে যান
```

### `git checkout` — পুরোনো, এখনো সব জায়গায় চলে

```bash
git checkout main             # branch বদলান
git checkout -b new-feature   # নতুন branch বানিয়ে সেখানে যান
git checkout -                # আগের branch-এ ফিরুন
git checkout abc123           # নির্দিষ্ট একটা commit-এ যান (detached HEAD)
```

### File-এর পরিবর্তন বাতিল করা

```bash
git restore file.txt          # আধুনিক — file-এর পরিবর্তন ফেলে দিন
git checkout -- file.txt      # পুরোনো — একই কাজ
```

> ⚠️ এই দুটোই save না করা পরিবর্তন **স্থায়ীভাবে মুছে ফেলে**। ফেরানোর উপায় নেই। চালানোর আগে দুবার ভাবুন।

### কোনটা ব্যবহার করব?

| কাজ | ব্যবহার করুন |
|---|---|
| Branch বদলানো | `git switch` |
| নতুন branch বানানো | `git switch -c` |
| File-এর পরিবর্তন বাতিল | `git restore` |
| পুরোনো tutorial দেখছেন | `git checkout` দেখলে ঘাবড়াবেন না — কাজ একই |

---

## একটা সাধারণ workflow

```bash
git status                       # ১. কী কী বদলেছে দেখুন
git diff                         # ২. ঠিক কী বদলেছে যাচাই করুন
git add .                        # ৩. পরিবর্তনগুলো stage করুন
git diff --staged                # ৪. কী commit হতে যাচ্ছে শেষবার দেখুন
git commit -m "Add user profile" # ৫. Commit করুন
git log --oneline                # ৬. ইতিহাসে দেখা যাচ্ছে কিনা নিশ্চিত হোন
```

---

## দ্রুত রেফারেন্স

```bash
# অবস্থা দেখা
git status                  # বর্তমান অবস্থা
git status -sb              # সংক্ষিপ্ত + branch

# পরিবর্তন stage করা
git add file.txt            # একটা file
git add .                   # সব
git add -p                  # অংশে অংশে বেছে নিয়ে
git restore --staged f.txt  # unstage করা

# Commit করা
git commit -m "message"     # commit
git commit -am "message"    # add + commit (tracked file)
git commit --amend          # শেষ commit ঠিক করা

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
git reset --soft HEAD~1     # commit বাতিল কিন্তু কাজ staged থাকবে
git reflog                  # HEAD-এর পুরো ইতিহাস — হারানো commit খুঁজুন
git reset --hard abc123     # reflog থেকে পাওয়া commit-এ ফিরে যান
git stash                   # কাজ না হারিয়ে সরিয়ে রাখুন
```

