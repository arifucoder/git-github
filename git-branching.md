# Git Branching

## Branch দেখা

```bash
git branch                  # সব local branch-এর list দেখাবে (* চিহ্ন = বর্তমান branch)
git branch -a               # local + remote সব branch দেখাবe
git branch -r               # শুধু remote branch দেখাবে
```

## নতুন Branch তৈরি করা

```bash
git branch feature-login            # নতুন branch তৈরি হবে, কিন্তু switch হবে না
git checkout -b feature-login       # নতুন branch তৈরি + সাথে সাথে switch হবে
git switch -c feature-login         # উপরেরটার আধুনিক বিকল্প (নতুন Git-এ recommended)
```

## Branch পরিবর্তন (Switch) করা

```bash
git checkout main           # main branch-এ চলে যাবে
git switch main             # একই কাজ, আধুনিক command
git switch -                # আগের branch-এ ফিরে যাবে (toggle)
```

## Show — একটা commit-এর details দেখা
```bash
git show abc123              # নির্দিষ্ট commit-এর পুরো info + diff দেখাবে
git show HEAD                 # সর্বশেষ commit দেখাবে
git show HEAD~2               # ২টা commit আগেরটা দেখাবে
git show abc123 --stat        # শুধু কোন কোন file পরিবর্তন হয়েছে তার সারমর্ম
git show abc123:file.txt      # নির্দিষ্ট commit-এ file.txt-এর content দেখাবে
```

## Branch Merge করা

```bash
git checkout main           # প্রথমে যে branch-এ merge করবেন সেখানে যান
git merge feature-login     # feature-login-এর সব কাজ main-এ যুক্ত হবে
```

## squash merge নিয়ে কাজ

```bash
git merge --squash feature   # feature branch-এর সব commit-এর change একসাথে stage করবে (commit history আসবে না)
git commit -m "message"      # merge --squash নিজে commit করে না, নিজে commit করতে হবে
```

## Cherry-pick — নির্দিষ্ট commit বেছে আনা
```bash
git cherry-pick abc123            # নির্দিষ্ট একটা commit অন্য branch থেকে current branch-এ আনা
git cherry-pick abc123 def456     # একাধিক commit একসাথে cherry-pick করা
git cherry-pick abc123^..def456   # একটা range-এর সব commit cherry-pick করা (abc123 বাদে, তার পরের থেকে def456 পর্যন্ত)

git cherry-pick --no-commit abc123   # cherry-pick করবে কিন্তু auto-commit করবে না, নিজে commit করতে হবে
git cherry-pick --continue            # conflict সমাধানের পর cherry-pick চালিয়ে যাওয়া
git cherry-pick --abort               # cherry-pick বাতিল করে আগের অবস্থায় ফেরা
```

## Branch Delete করা

```bash
git branch -d feature-login     # merge হয়ে গেলে delete হবে (safe)
git branch -D feature-login     # merge না হলেও জোর করে delete হবে (সাবধান!)
git push origin --delete feature-login    # remote branch delete করা
```

## Branch-এর নাম পরিবর্তন

```bash
git branch -m new-name              # বর্তমান branch-এর নাম বদলাবে
git branch -m old-name new-name     # নির্দিষ্ট branch-এর নাম বদলাবে
```

## Remote Branch নিয়ে কাজ

```bash
git push -u origin feature-login    # নতুন branch remote-এ push করা (-u = tracking set হবে)
git fetch origin                    # remote-এর সব update নামাবে, কিন্তু merge করবে না
git pull origin main                # remote main-এর update এনে merge করবে
git checkout -b local-name origin/remote-name    # remote branch থেকে local branch তৈরি
```


## দরকারি টিপস

```bash
git log --oneline --graph --all     # সব branch-এর history গ্রাফ আকারে দেখাবে
git stash                           # branch switch করার আগে অসম্পূর্ণ কাজ সাময়িক জমা রাখা
git stash pop                       # জমা রাখা কাজ ফিরিয়ে আনা
```

**মনে রাখার সারমর্ম:** branch তৈরি করুন → কাজ করুন → main-এ ফিরে merge করুন → branch delete করুন।