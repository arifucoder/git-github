# নিজের Gist-এর মধ্যে কীভাবে search করবেন

> শুধুমাত্র **আপনার নিজের** gist-গুলোর মধ্যে খোঁজার সব উপায় — browser, search qualifier, `gh` CLI এবং API সহ।

---

## দ্রুত উত্তর (TL;DR)

| আপনি যা চান | সবচেয়ে ভালো উপায় |
|---|---|
| শুধু নিজের **public** gist-এ keyword খোঁজা | `https://gist.github.com/search?q=user:USERNAME+keyword` |
| **secret gist সহ** সব gist-এ খোঁজা | `gh` CLI বা GitHub API (নিচে দেখুন) |
| নিজের gist-এর list ঝটপট দেখা | `https://gist.github.com/USERNAME` |
| Local-এ পুরো content grep করা | সব gist clone করে `grep -r` |

> ⚠️ **সবচেয়ে গুরুত্বপূর্ণ কথা:** Gist-এর ওয়েব search **secret (unlisted) gist খুঁজে পায় না**। Secret gist-এ search করতে হলে `gh` CLI বা API ব্যবহার করতেই হবে।

---

## ১. Browser দিয়ে search (public gist)

### ধাপ

1. [gist.github.com](https://gist.github.com) খুলুন এবং login করুন
2. উপরের **search box**-এ keyword লিখুন
3. Query-র সাথে `user:` qualifier যোগ করুন যাতে শুধু আপনার gist আসে

### উদাহরণ

```
user:yourusername docker
```

এতে শুধু আপনার gist-গুলোর মধ্যে `docker` শব্দটা খোঁজা হবে।

সরাসরি URL দিয়েও যাওয়া যায়:

```
https://gist.github.com/search?q=user%3Ayourusername+docker
```

> 💡 `%3A` হলো `:` এর URL-encoded রূপ। Browser-এ সাধারণভাবে `user:yourusername docker` টাইপ করলেই কাজ করবে।

---

## ২. কাজে লাগার মতো search qualifier

Gist search-এ নিচের qualifier-গুলো ব্যবহার করা যায়। একাধিক qualifier একসাথে মেলানো যায়।

| Qualifier | কাজ | উদাহরণ |
|---|---|---|
| `user:` | নির্দিষ্ট user-এর gist | `user:torvalds` |
| `language:` | নির্দিষ্ট programming language | `language:python` |
| `extension:` | নির্দিষ্ট file extension | `extension:sh` |
| `fork:true` | fork করা gist-ও দেখাবে | `fork:true user:me` |
| `filename:` | নির্দিষ্ট নামের file | `filename:notes.md` |
| `"..."` | exact phrase খোঁজা | `"connection refused"` |

### Combine করার উদাহরণ

```
user:yourusername language:python asyncio
```
→ আপনার Python gist-গুলোর মধ্যে `asyncio` আছে এমন gist।

```
user:yourusername extension:md "meeting notes"
```
→ আপনার `.md` gist-এর মধ্যে ঠিক `meeting notes` phrase-টা আছে এমন gist।

---

## ৩. নিজের gist-এর তালিকা ব্রাউজ করা

Search না করে সরাসরি list দেখতে চাইলে:

```
https://gist.github.com/yourusername
```

এই পাতায় আপনার **public + secret** দুটোই দেখা যাবে (আপনি login করা থাকলে)। পাতার search box দিয়ে **description আর filename** ধরে filter করা যায় — তবে এটা file-এর **ভেতরের content** খোঁজে না।

তিনটা tab পাবেন:
- **All gists** — সবগুলো
- **Starred** — যেগুলোতে star দিয়েছেন
- Sort করার option: *Recently created* / *Recently updated*

---

## ৪. `gh` CLI দিয়ে (secret gist সহ — সবচেয়ে শক্তিশালী)

GitHub-এর official CLI। এটাই secret gist-এ খোঁজার সবচেয়ে সহজ উপায়।

### Install

```bash
# Windows
winget install --id GitHub.cli

# macOS
brew install gh

# Ubuntu/Debian
sudo apt install gh
```

### Login

```bash
gh auth login
```

### সব gist-এর list

```bash
gh gist list --limit 100
```

Output-এ পাবেন: gist ID, description, file সংখ্যা, public/secret status, update time।

### শুধু secret gist

```bash
gh gist list --secret --limit 100
```

### Description ধরে filter

```bash
gh gist list --limit 100 | grep -i "docker"
```

### একটা gist-এর content দেখা

```bash
gh gist view <GIST_ID>
```

### File-এর **ভেতরের content** ধরে খোঁজা (আসল কাজটা)

`gh gist list` শুধু metadata দেয়, content দেয় না। তাই content খুঁজতে একটা ছোট loop চালাতে হবে:

```bash
# Linux / macOS / Git Bash
gh gist list --limit 100 | while IFS=$'\t' read -r id rest; do
  if gh gist view "$id" --raw 2>/dev/null | grep -iq "SEARCH_TERM"; then
    echo "✅ Found in: https://gist.github.com/$id  — $rest"
  fi
done
```

`SEARCH_TERM` এর জায়গায় আপনার keyword বসান।

**PowerShell version:**

```powershell
gh gist list --limit 100 | ForEach-Object {
    $id = ($_ -split "`t")[0]
    $content = gh gist view $id --raw 2>$null
    if ($content -match "SEARCH_TERM") {
        Write-Host "✅ Found in: https://gist.github.com/$id"
    }
}
```

---

## ৫. GitHub API দিয়ে

Script লিখতে চাইলে বা automate করতে চাইলে API সবচেয়ে flexible।

### নিজের সব gist-এর list (secret সহ)

```bash
gh api /gists --paginate
```

অথবা `curl` দিয়ে (একটা Personal Access Token লাগবে):

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     -H "Accept: application/vnd.github+json" \
     https://api.github.com/gists
```

### `jq` দিয়ে সুন্দর করে filter

```bash
# description-এ keyword আছে এমন gist
gh api /gists --paginate | jq -r '
  .[] | select(.description | test("docker"; "i")) |
  "\(.html_url)  —  \(.description)"
'
```

```bash
# নির্দিষ্ট filename খোঁজা
gh api /gists --paginate | jq -r '
  .[] | select(.files | keys[] | test("\\.md$")) | .html_url
'
```

> ℹ️ API-এর gist list-এ file-এর content আসে না (শুধু metadata আর `raw_url`)। Content লাগলে `raw_url` fetch করতে হবে অথবা `gh api /gists/<id>` কল করতে হবে।

---

## ৬. সব gist local-এ নামিয়ে `grep` করা

সবচেয়ে নিশ্চিত পদ্ধতি — একবার সব নামিয়ে নিন, তারপর যত খুশি offline search করুন।

```bash
mkdir -p ~/my-gists && cd ~/my-gists

# সব gist clone করুন
gh gist list --limit 200 | cut -f1 | while read -r id; do
  git clone "https://gist.github.com/$id.git" "$id" 2>/dev/null
done

# এখন যেকোনো keyword খুঁজুন
grep -rin "SEARCH_TERM" .
```

**সুবিধা:**
- Secret gist-ও চলে আসে
- Regex দিয়ে খোঁজা যায়, খুব দ্রুত
- Offline কাজ করে
- `ripgrep` (`rg`) থাকলে আরও দ্রুত: `rg -i "SEARCH_TERM"`

**পরে update করতে:**

```bash
for d in */; do (cd "$d" && git pull -q); done
```

---

## ৭. VS Code extension

Extension marketplace-এ **GitHub Gist** / **Gist Pad** ধরনের extension পাওয়া যায়। এগুলো দিয়ে:

- Sidebar-এ নিজের সব gist (secret সহ) দেখা যায়
- Editor-এর ভেতর থেকেই open, edit, save করা যায়
- File খুলে ফেললে VS Code-এর নিজস্ব search (`Ctrl+Shift+F`) দিয়ে খোঁজা যায়

---

## সীমাবদ্ধতা ও সাধারণ ভুল

| সমস্যা | কারণ / সমাধান |
|---|---|
| Secret gist search-এ আসছে না | এটাই স্বাভাবিক — gist search secret gist index করে না। `gh` CLI বা API ব্যবহার করুন |
| নতুন বানানো gist search-এ নেই | Search index হতে কিছুটা সময় লাগতে পারে |
| GitHub-এর main code search (`github.com/search`) gist দেখাচ্ছে না | Gist আলাদা — অবশ্যই `gist.github.com/search` ব্যবহার করতে হবে |
| খুব বড় file-এর ভেতরের content search-এ আসছে না | বড় file GitHub index না-ও করতে পারে |
| `gh gist list` সব gist দেখাচ্ছে না | Default limit ছোট — `--limit 100` বা `--limit 200` দিন |

---

## চিট-শিট

```bash
# Browser
https://gist.github.com/search?q=user:USERNAME+keyword

# নিজের সব gist-এর তালিকা
gh gist list --limit 100

# শুধু secret
gh gist list --secret

# Description ধরে
gh gist list --limit 100 | grep -i "keyword"

# Content ধরে (পুরো search)
gh gist list --limit 100 | while IFS=$'\t' read -r id rest; do
  gh gist view "$id" --raw 2>/dev/null | grep -iq "keyword" && \
    echo "https://gist.github.com/$id — $rest"
done

# API + jq
gh api /gists --paginate | jq -r '.[] | select(.description | test("keyword";"i")) | .html_url'
```