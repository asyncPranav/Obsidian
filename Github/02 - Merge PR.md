
---

# 🔹 Step 1: Get the contributor’s branch locally

```sh
git checkout -b contributor-branch main
```

- `checkout -b contributor-branch main` → create a new branch called `contributor-branch`, starting from `main`.
    
- This is where you’ll pull the contributor’s code.
    
```sh
git pull https://github.com/username/forked-repo.git feature-branch
```
		
- Pulls the `feature-branch` from the contributor’s fork.
    
- Now your local branch (`contributor-branch`) has all the changes from their PR.
    

---

# 🔹 Step 2: Review the changes

```sh
git diff main...contributor-branch
```

- `git diff` shows what’s different between `main` and the contributor’s branch.
    
- `...` means compare both branches from their common ancestor.
    
- Use this to check exactly what new lines of code will come in.
    

👉 You can also run tests or build commands here to verify the PR.

---

# 🔹 Step 3: Merge the PR into main

### Option 1: **Normal merge**

```sh
git checkout main
git merge contributor-branch
```

- Moves `main` forward to include the contributor’s work.
    
- If fast-forward possible, Git just moves the pointer.
    

---

### Option 2: **No fast-forward merge**

```sh
git checkout main
git merge --no-ff contributor-branch
```

- `--no-ff` = always create a merge commit.
    
- Keeps branch history visible (“this code came from PR X”).
    
- Good for open-source projects where you want clear history.
    

---

### Option 3: **Squash merge**

```sh
git checkout main
git merge --squash contributor-branch
git commit -m "Add feature XYZ from contributor"
```

- Combines all commits from contributor into **one clean commit**.
    
- History looks neat, but you lose the original commit breakdown.
    

---

### Option 4: **Rebase merge** (rare for PRs, but possible)

```sh
git checkout contributor-branch
git rebase main
git checkout main
git merge contributor-branch
```

- Moves contributor’s commits **on top of main**, as if written in sequence.
    
- Creates a clean linear history, but can be risky (conflicts).
    

---

# 🔹 Step 4: Push merged changes

```sh
git push origin main
```

- Updates the **official repo on GitHub** with the merged changes.
    

---

# 🔹 Step 5: Cleanup (optional)

```sh
git branch -d contributor-branch
git push origin --delete contributor-branch
```

- Deletes the temporary branch you used to test & merge.
    
- Safe, since the work is already in `main`.
    

---

# ✅ Example Flow (Owner merging PR)

```sh
git checkout -b contributor-branch main
git pull https://github.com/username/forked-repo.git feature-branch
git diff main...contributor-branch   # review code
git checkout main
git merge --no-ff contributor-branch # merge style
git push origin main                 # update GitHub
git branch -d contributor-branch     # cleanup
```

---

⚡ On GitHub UI, when you click **“Merge Pull Request”**, it actually runs one of these commands behind the scenes (depending on whether you choose _Merge, Squash, or Rebase_).