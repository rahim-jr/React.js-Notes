# Git & GitHub — Class 25

---

## 1. What is Git?

**Git** is a **version control system** — a tool that tracks every change you make to your code over time.

Think of it like a **save history** for your project:
- You can see every change ever made
- You can go back to any previous version
- You can work on new features without breaking the working code
- Multiple people can work on the same project without overwriting each other

> 💡 Git runs **locally** on your computer. It does not require the internet.

---

## 2. What is GitHub?

**GitHub** is a website (a cloud database) where you can **store and share your Git repositories** online.

| | Git | GitHub |
|---|---|---|
| **What it is** | A tool installed on your computer | A website on the internet |
| **Purpose** | Tracks changes locally | Stores your code in the cloud |
| **Needs internet?** | ❌ No | ✅ Yes |
| **Analogy** | Your local save files | Google Drive for code |

> 🔑 Git is the tool. GitHub is the storage. They work together but are not the same thing.

---

## 3. Install Git

Download and install Git from: [git-scm.com](https://git-scm.com)

After installing, verify it works:

```bash
git --version
```

You should see something like: `git version 2.x.x`

---

## 4. Key Terms

| Term | Meaning |
|---|---|
| **Repository (Repo)** | A folder tracked by Git — your project's database of all changes |
| **Commit** | A saved snapshot of your project at a specific point in time |
| **Branch** | An independent line of development — lets you work on features without affecting main code |
| **Remote** | The online version of your repository (usually on GitHub) |
| **Clone** | Download a copy of a repository from GitHub to your computer |
| **Push** | Upload your local commits to GitHub |
| **Pull** | Download the latest changes from GitHub to your computer |
| **Fetch** | Download all branches from the remote — "give me everyone's branches" |
| **Merge** | Combine two branches together |
| **Staging Area** | A holding zone where you prepare changes before committing them |

---

## 5. Setting Up Git — First Time Only

Before your first commit, tell Git who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

> 💡 This information gets attached to every commit you make so others know who made the change.

---

## 6. Creating a New Repository on GitHub

### Step 1 — Go to GitHub
Visit [github.com](https://github.com) and log in (or create an account).

### Step 2 — Create a New Repository
1. Click the **"+"** button in the top right corner
2. Select **"New repository"**
3. Give it a name (e.g., `my-react-app`)
4. Choose **Public** or **Private**
5. Click **"Create repository"**

GitHub will give you a URL like:
```
https://github.com/your-username/my-react-app.git
```

---

## 7. Setting Up a Local Project with Git

### Step 1 — Create a Local Folder

```bash
mkdir my-project
cd my-project
```

Or open an existing project folder in VS Code.

### Step 2 — Check if Git Already Exists

```bash
git status
```

- If Git is initialized: you'll see info about tracked/untracked files
- If Git is **not** initialized: you'll see `fatal: not a git repository`

### Step 3 — Initialize Git

```bash
git init
```

This creates a hidden `.git` folder inside your project — this is where Git stores all its tracking data.

### Step 4 — Verify the `.git` Folder Was Created

```bash
ls        # lists all visible files and folders
ls -a     # lists ALL files including hidden ones (starts with .)
```

You should now see a `.git` folder in the list.

---

## 8. The Basic Git Workflow

This is the cycle you'll repeat for every change you make:

```
Make changes to your code
        ↓
Stage the changes (tell Git what to include)
        ↓
Commit the changes (save a snapshot)
        ↓
Push to GitHub (upload to the cloud)
```

### Step-by-Step Commands

```bash
# 1. See what has changed
git status

# 2. Stage all changes (add them to the "staging area")
git add .
# OR stage a specific file:
git add src/App.js

# 3. Commit the staged changes with a message
git commit -m "Add counter feature to App.js"

# 4. Push to GitHub
git push origin main
```

---

## 9. Connecting Your Local Repo to GitHub

After running `git init` locally, link it to your GitHub repository:

```bash
# Add the remote GitHub repo as the "origin"
git remote add origin https://github.com/your-username/my-react-app.git

# Push your code and set "origin/main" as the default
git push -u origin main
```

> 💡 `origin` is just a nickname for the GitHub URL. You can name it anything, but `origin` is the convention.

---

## 10. Essential Git Commands Reference

| Command | What It Does |
|---|---|
| `git init` | Initialize a new Git repository in the current folder |
| `git status` | Show which files have changed, staged, or are untracked |
| `git add .` | Stage all changes in the current directory |
| `git add <file>` | Stage a specific file |
| `git commit -m "message"` | Save a snapshot with a descriptive message |
| `git log` | View the history of all commits |
| `git push origin main` | Upload commits to the `main` branch on GitHub |
| `git pull origin main` | Download the latest changes from GitHub |
| `git fetch` | Download all branches from the remote (without merging) |
| `git clone <url>` | Download a full copy of a GitHub repo to your computer |
| `ls` | List all visible files and folders |
| `ls -a` | List ALL files including hidden ones (like `.git`) |
| `pwd` | Print present working directory |

---

## 11. What is a Branch?

A **branch** is like a parallel copy of your project where you can experiment safely.

```
main branch:      A --- B --- C
                              |
feature branch:               D --- E --- F
```

- `main` is your stable, working code
- You create a new branch to work on a feature
- When the feature is done, you **merge** it back into `main`

### Branch Commands

```bash
# Create a new branch
git branch feature/search-bar

# Switch to that branch
git checkout feature/search-bar

# Create AND switch in one command
git checkout -b feature/search-bar

# List all branches
git branch

# Merge a branch into main
git checkout main
git merge feature/search-bar
```

---

## 12. What `git fetch` Actually Means

```bash
git fetch
```

`git fetch` downloads **all branches** from the remote (GitHub) to your local machine — but it does **not** automatically merge any changes into your current code.

Think of it as: *"Show me what everyone else has been working on."*

To actually apply those changes to your code, you then run `git merge` or use `git pull` (which does both fetch + merge in one step).

---

## 13. `.gitignore` — What Not to Track

Some files should **never** be pushed to GitHub:
- `node_modules/` — too large, can always be regenerated with `npm install`
- `.env` — contains secret API keys

Create a `.gitignore` file at the root of your project:

```
# .gitignore
node_modules/
.env
build/
.DS_Store
```

Any file or folder listed here will be **completely ignored** by Git.

---

## Quick Summary

```
Git    → tracks changes on your local computer
GitHub → stores your code in the cloud

git init          → start tracking a folder
git status        → see what changed
git add .         → stage all changes
git commit -m ""  → save a snapshot
git push          → upload to GitHub
git pull          → download from GitHub
git fetch         → get all remote branches (no merge)

.gitignore        → list files Git should never track
```

> 🔑 **The golden rule:** Commit often with clear messages. Every commit should represent one logical change.