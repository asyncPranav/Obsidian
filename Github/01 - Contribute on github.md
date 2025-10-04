
---

# 🔹 1. Fork the Repository

- Go to the GitHub repo you want to contribute to.
    
- Click **Fork** → this creates a **copy of the repo under your GitHub account**.
    
- You will work on **your fork**, not directly on the original repo.
    

---

# 🔹 2. Clone Your Fork Locally

```sh
git clone https://github.com/your-username/repo-name.git
cd repo-name
```

- Now you have a **local copy of your fork**.
    

---

# 🔹 3. Add Original Repo as Remote

This is important to **keep your fork up-to-date** with the original project.

```sh
git remote add upstream https://github.com/original-username/repo-name.git
```

Check remotes:

```sh
git remote -v
```

Output:

```sh
origin    https://github.com/your-username/repo-name.git (fetch)
origin    https://github.com/your-username/repo-name.git (push)
upstream  https://github.com/original-username/repo-name.git (fetch)
upstream  https://github.com/original-username/repo-name.git (push)
```

---

# 🔹 4. Create a New Branch

**Never work directly on `main`**. Always create a branch for your feature/fix:

```sh
git checkout -b fix-bug-123
```

---

# 🔹 5. Make Changes Locally

- Edit/add files.
    
- Stage and commit changes with clear message:
    

```sh
git add .
git commit -m "Fix bug in login flow"
```

---

# 🔹 6. Keep Your Branch Up-to-Date

Before pushing, make sure your fork is **in sync with upstream main**:

```sh
git fetch upstream
git checkout main
git merge upstream/main
```

---

# 🔹 7. Push Your Branch to Your Fork

`git push origin fix-bug-123`

---

# 🔹 8. Open a Pull Request (PR)

- Go to your fork on GitHub.
    
- Click **Compare & Pull Request**.
    
- Base: `original repo/main`
    
- Compare: `your fork/fix-bug-123`
    
- Add **title and description** (describe what you changed and why).
    
- Submit PR.
    

---

# 🔹 9. Respond to Review

- Maintainers might comment or request changes.
    
- Make changes locally on the **same branch**, commit, push again:
    

`git add . git commit -m "Address review comments" git push origin fix-bug-123`

- The PR updates automatically.
    

---

# 🔹 10. After PR is Merged

- Pull latest changes from original repo to your local main:
    

`git checkout main git fetch upstream git merge upstream/main`

- Delete feature branch locally and on your fork:
    

`git branch -d fix-bug-123 git push origin --delete fix-bug-123`

---

✅ **Summary Workflow**

`Fork → Clone → Create Branch → Work & Commit → Push → PR → Review → Merge → Sync`

---

💡 Pro Tips:

- Use **small, focused PRs**. One PR = one feature/fix.
    
- Write **clear commit messages**.
    
- Always **sync with upstream** before starting new work.