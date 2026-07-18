# 📝 Semantic Commit Messages

> কমিট মেসেজ লেখার একটি সুন্দর ও গোছানো নিয়ম, যা দেখলেই বোঝা যায় কমিটে কী পরিবর্তন করা হয়েছে।

---

## 🎯 কেন Semantic Commit ব্যবহার করবেন?

- কমিট হিস্টোরি পড়া সহজ হয়
- কোন কমিটে কী পরিবর্তন হয়েছে তা এক নজরে বোঝা যায়
- Changelog স্বয়ংক্রিয়ভাবে তৈরি করা যায়
- টিমের সবাই একই নিয়ম মেনে চলে, ফলে সামঞ্জস্য বজায় থাকে
- `semantic-release` এর মতো টুল দিয়ে version bump অটোমেট করা যায়

---

## 🧱 ফরম্যাট

```
<type>(<scope>): <subject>
```

- **type** → পরিবর্তনের ধরন (আবশ্যক)
- **scope** → কোন অংশে পরিবর্তন হয়েছে (ঐচ্ছিক)
- **subject** → সংক্ষিপ্ত বর্ণনা (আবশ্যক)

**উদাহরণ:**

```
feat(auth): add google login support
```

---

## 🏷️ Type-গুলোর তালিকা

| Type | কখন ব্যবহার করবেন | উদাহরণ |
|------|-------------------|---------|
| `feat` | নতুন ফিচার যোগ করলে | `feat: add dark mode toggle` |
| `fix` | বাগ ঠিক করলে | `fix: resolve login crash on empty password` |
| `docs` | শুধু ডকুমেন্টেশন পরিবর্তন করলে | `docs: update README installation guide` |
| `style` | কোডের ফরম্যাটিং পরিবর্তন (লজিকে কোনো প্রভাব নেই) | `style: fix indentation in header.js` |
| `refactor` | কোড পুনর্গঠন — নতুন ফিচারও না, বাগ ফিক্সও না | `refactor: simplify user validation logic` |
| `perf` | পারফরম্যান্স উন্নত করলে | `perf: optimize image loading` |
| `test` | টেস্ট যোগ বা আপডেট করলে | `test: add unit tests for cart module` |
| `build` | বিল্ড সিস্টেম বা ডিপেন্ডেন্সি পরিবর্তন করলে | `build: upgrade webpack to v5` |
| `ci` | CI/CD কনফিগারেশন পরিবর্তন করলে | `ci: add GitHub Actions workflow` |
| `chore` | অন্যান্য টুকিটাকি কাজ (src বা test-এ পরিবর্তন নেই) | `chore: update .gitignore` |
| `revert` | আগের কোনো কমিট revert করলে | `revert: revert commit abc1234` |

---

## 🔍 Scope ব্যবহার

Scope দিয়ে বোঝানো হয় প্রজেক্টের **কোন অংশে** পরিবর্তন হয়েছে:

```
feat(api): add user endpoint
fix(ui): correct button alignment
docs(readme): add contribution guide
```

---

## ✍️ Subject লেখার নিয়ম

- ✅ **Imperative mood** ব্যবহার করুন → `add` লিখুন, `added` বা `adds` নয়
- ✅ ছোট হাতের অক্ষরে শুরু করুন
- ✅ ৫০ অক্ষরের মধ্যে রাখার চেষ্টা করুন
- ❌ শেষে ফুলস্টপ (`.`) দেবেন না

```
✅ feat: add payment gateway
❌ feat: Added payment gateway.
```

---

## 📄 Body ও Footer (ঐচ্ছিক)

বড় পরিবর্তনের ক্ষেত্রে বিস্তারিত ব্যাখ্যার জন্য body এবং footer ব্যবহার করা যায়:

```
fix(auth): prevent session timeout on active users

Session was expiring even when the user was actively
using the app. Added an activity listener to refresh
the token automatically.

Closes #142
```

- **Body** → কেন এবং কীভাবে পরিবর্তন করা হয়েছে
- **Footer** → issue reference (`Closes #142`) বা breaking change

---

## 💥 Breaking Change

API বা আচরণে বড় ধরনের পরিবর্তন হলে `!` চিহ্ন বা `BREAKING CHANGE:` ফুটার ব্যবহার করুন:

```
feat(api)!: change response format to JSON:API spec

BREAKING CHANGE: all endpoints now return data in
JSON:API format. Clients must update their parsers.
```

---

## ⚡ কুইক চিটশিট

```
feat:     নতুন ফিচার
fix:      বাগ ফিক্স
docs:     ডকুমেন্টেশন
style:    ফরম্যাটিং
refactor: কোড পুনর্গঠন
perf:     পারফরম্যান্স
test:     টেস্ট
build:    বিল্ড/ডিপেন্ডেন্সি
ci:       CI/CD
chore:    টুকিটাকি কাজ
revert:   কমিট revert
```

---

## 🔗 আরও জানতে

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)

---

> 💡 **টিপ:** প্রতিটি কমিট ছোট রাখুন এবং একটি কমিটে একটিমাত্র logical change রাখুন।