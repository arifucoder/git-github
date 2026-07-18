# GitHub Repository-তে Collaborate করার সম্পূর্ণ গাইড

কোনো repository-তে collaborate করার **২টি উপায়** আছে:

1. **সরাসরি repo-তে access দেওয়া**
2. **Repo-টি fork করে নিয়ে কাজ করা**

---

## পদ্ধতি ১: সরাসরি Access দেওয়া

Repository-র মালিক নিচের ধাপে collaborator যুক্ত করবেন:

> **Settings → Collaborators and Teams → Add People**

এখান থেকে GitHub username/email দিয়ে collaborator add করলেই সরাসরি access পাওয়া যাবে।

---

## পদ্ধতি ২: Fork করে কাজ করা

### ধাপ ১: Fork করা

- Original repo-তে গিয়ে **Fork** বাটনে ক্লিক করে repo-টি নিজের অ্যাকাউন্টে fork করতে হবে।

### ধাপ ২: Clone করে PC-তে নিয়ে আসা

```bash
git clone folder-url
```

### ধাপ ৩: Remote সেটআপ করা

Remote-গুলো দেখতে:

```bash
git remote -v
```

এখানে `origin` দেখা যাবে। এবার এই `origin`-এর নাম rename করে **source folder-এর মালিকের username** দিয়ে রাখতে পারি:

```bash
git remote rename origin bongodev
```

এবার আমার **fork করা repo-টি** নতুন remote হিসেবে add করব:

```bash
git remote add arifucoder amar-fork-kora-git-url
```

> 📌 **মনে রাখবেন:**
> - `bongodev` = original repo (upstream)
> - `arifucoder` = আমার fork করা repo

### ধাপ ৪: Master/Main Branch আপডেট করা

⚠️ **আমরা কখনোই master/main branch-এ কাজ করব না!** আলাদা branch বানিয়ে সেখানে কাজ করব।

প্রথমে original repo থেকে fetch করব:

```bash
git fetch bongodev
```

এবার original repo দিয়ে আমার local master/main branch আপডেট করে নেব:

```bash
git reset --hard bongodev/master
```

> ⚠️ এই command দিলে আমার local master/main branch-এ যা আছে **সব ফেলে দিয়ে** `bongodev`-এ যা আছে তা দিয়ে আপডেট করে দেবে।

### ধাপ ৫: নতুন Branch তৈরি ও Checkout করা

Branch create + checkout একসাথে:

```bash
git switch -c something-branch
```

### ধাপ ৬: কাজ করা ও Commit করা

- আমাদের মতো করে code বা file আপডেট করব
- Save করে commit করব

Commit message **অবশ্যই** এই ফরম্যাটে লিখতে হবে:

```bash
git commit -m "[arifucoder] git message"
```

### ধাপ ৭: নিজের Repo-তে Push করা

```bash
git push amar-repo-remote-origin-name branch-name
```

উদাহরণ:

```bash
git push arifucoder something-branch
```

### ধাপ ৮: Pull Request তৈরি করা

Original repo-তে গিয়ে:

1. **Pull Request** option → **New Pull Request**
2. **Compare across forks**-এ ক্লিক
3. সেট করব:
   - **Base:** original repo + master/main branch
   - **Compare:** আমার repo + আমার কাজ করা branch
4. **Create Pull Request**-এ ক্লিক
5. Title + Description দেব:
   - **Title:** `[amar-github-user-name] message`
   - **Description:** বিস্তারিত message
6. Finally **Create Pull Request**-এ ক্লিক

✅ এবার **Pull Requests** ট্যাবে গেলে আমার pull request দেখতে পাব।

### ধাপ ৯: Review

- Pull request আসলে **team থেকে reviewer assign** করা যায়।
- যেখানে feed থাকবে সেখানে **`+` sign**-এ ক্লিক করে message/comment দিতে হবে।

---

## Merge Conflict Resolve করার পদ্ধতি

১. VS Code-এর terminal-এ যাব

২. Original repo থেকে fetch করব:

```bash
git fetch bongodev
```

৩. Rebase করব:

```bash
git rebase bongodev/master
```

৪. এবার VS Code-এ **merge conflict** দেখাবে।

৫. Git icon-এ ক্লিক করে **Merge Changes**-এর under-এ file-গুলো এক এক করে ক্লিক করে **প্রয়োজন মতো আপডেট** করব।

৬. সব ঠিক করার পর:

```bash
git add .
```

৭. Rebase continue করব:

```bash
git rebase --continue
```

> এই command দিলে vim/nano-তে commit message edit করতে বলবে — edit করে save করব।

৮. **অবশ্যই** log চেক করব:

```bash
git log
```

৯. নিজের repo-তে force push করব:

```bash
git push arifucoder branch-name -f
```

✅ এবার pull request **Ready to Merge** হয়ে যাবে! 🎉

---

## Quick Reference (Command Summary)

| কাজ | Command |
|------|---------|
| Clone করা | `git clone folder-url` |
| Remote দেখা | `git remote -v` |
| Remote rename | `git remote rename origin bongodev` |
| Remote add | `git remote add arifucoder fork-url` |
| Fetch করা | `git fetch bongodev` |
| Local master আপডেট | `git reset --hard bongodev/master` |
| Branch create + switch | `git switch -c branch-name` |
| Commit | `git commit -m "[username] message"` |
| Push | `git push arifucoder branch-name` |
| Rebase | `git rebase bongodev/master` |
| Rebase continue | `git rebase --continue` |
| Force push | `git push arifucoder branch-name -f` |