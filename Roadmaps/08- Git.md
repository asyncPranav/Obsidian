
# **Git Mastery Roadmap – Chapter-wise**

---

## **Chapter 1: Review & Solidify Basics**

**Goal:** Make sure you have rock-solid understanding of local Git workflow.

**Topics:**

1. Recap: Git lifecycle (working directory → staging → commit → branch).
    
2. Local commits: creating, amending, undoing (`git commit --amend`).
    
3. Branching basics: create, switch, delete branches (`git branch`, `git checkout`, `git switch`).
    
4. Merging basics: `git merge`, fast-forward vs merge commits.
    
5. Inspecting history: `git log`, `git log --oneline --graph --decorate --all`.
    
6. Resetting and reverting commits:
    
    - `git reset --soft/mixed/hard`
        
    - `git revert <commit>`
        

**Exercises:**

- Create a branch, make 3–4 commits, merge into main.
    
- Undo last commit using `reset` and `revert`.
    

---

## **Chapter 2: Remote Repositories & Collaboration**

**Goal:** Learn to safely collaborate and contribute to any remote repo.

**Topics:**

1. Remote concepts: `origin`, `upstream`.
    
2. Forking, cloning, and setting upstream.
    
3. Push and pull: `git push`, `git pull`, `git fetch`.
    
4. Tracking remote branches: `git branch -u`.
    
5. PR workflow: fork → branch → push → PR → review → merge.
    
6. Keeping your fork/branch up-to-date with upstream (`fetch`, `merge`, `rebase`).
    

**Exercises:**

- Fork an open-source project, make a branch, add a feature, push, open PR.
    
- Sync your fork with the upstream repo.
    

---

## **Chapter 3: Merge Conflicts & Conflict Resolution**

**Goal:** Handle conflicts confidently.

**Topics:**

1. Why conflicts happen.
    
2. Conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
    
3. Resolving conflicts manually.
    
4. Using merge tools (VSCode, Meld, KDiff3).
    
5. Best practices to avoid conflicts.
    

**Exercises:**

- Create two branches modifying the same line, merge, resolve conflict.
    
- Simulate conflict with upstream changes and your fork.
    

---

## **Chapter 4: Advanced Branching & Rebasing**

**Goal:** Clean and maintain a neat history.

**Topics:**

1. Rebase basics (`git rebase main`).
    
2. Interactive rebase (`git rebase -i HEAD~3`)
    
    - Squash commits
        
    - Reorder commits
        
    - Edit commit messages
        
3. Branching strategies: feature branch, develop branch, release branch.
    
4. Cherry-picking (`git cherry-pick <hash>`).
    

**Exercises:**

- Rebase feature branch onto updated main.
    
- Squash multiple commits into one.
    
- Cherry-pick a commit to another branch.
    

---

## **Chapter 5: Stashing & Temporary Changes**

**Goal:** Manage work-in-progress efficiently.

**Topics:**

1. `git stash` → save uncommitted changes.
    
2. `git stash list`, `git stash show`, `git stash apply`, `git stash pop`.
    
3. Stashing specific files.
    
4. Applying stash to different branches.
    

**Exercises:**

- Start working on a new feature, stash changes, switch branch, apply stash.
    
- Resolve conflicts in stashed changes.
    

---

## **Chapter 6: Tags, Releases & Versioning**

**Goal:** Learn to mark important commits and release versions.

**Topics:**

1. Creating tags: lightweight vs annotated.
    
    - `git tag v1.0`
        
    - `git tag -a v1.0 -m "Release 1.0"`
        
2. Pushing tags to remote: `git push origin v1.0`
    
3. Deleting tags.
    
4. Using tags for software releases.
    

**Exercises:**

- Tag last stable commit as `v1.0`.
    
- Push tags and verify on GitHub.
    

---

## **Chapter 7: Advanced Log & History Inspection**

**Goal:** Read and analyze repo history efficiently.

**Topics:**

1. Short log: `git log --oneline`.
    
2. Graphical log: `git log --graph --decorate --all`.
    
3. Filter commits by author: `git log --author="Name"`.
    
4. Show changes per commit: `git log -p`, `git log --stat`.
    
5. Searching in commit messages: `git log --grep="bug"`.
    

**Exercises:**

- Analyze history of a repo, find commits by a specific author.
    
- List commits affecting a specific file.
    

---

## **Chapter 8: Git Aliases & Productivity**

**Goal:** Make Git commands faster and more intuitive.

**Topics:**

1. Config basics: `git config --global`.
    
2. Aliases:
    
    `git config --global alias.st status git config --global alias.co checkout git config --global alias.lg "log --oneline --graph --decorate --all"`
    
3. Global config for editor, pager, colors.
    

**Exercises:**

- Create aliases for your most used commands.
    
- Customize Git to open VSCode by default.
    

---

## **Chapter 9: Advanced Workflows & Open Source Contribution**

**Goal:** Work confidently in real-world projects.

**Topics:**

1. Feature branch workflow (GitHub Flow).
    
2. Fork & PR workflow (already learned, refine it).
    
3. Rebasing vs merging in team projects.
    
4. Handling long-running branches.
    
5. Reviewing PRs from others.
    
6. Contribution etiquette: small PRs, meaningful commits, clear messages.
    

**Exercises:**

- Contribute to a real open-source repo (small fix or doc change).
    
- Review someone else’s PR and suggest improvements.
    

---

## **Chapter 10: Recovery & Debugging**

**Goal:** Safely recover lost work and debug mistakes.

**Topics:**

1. Undo last commits safely (`git reset`, `git revert`).
    
2. Recover deleted branch: `git reflog`
    
3. Recover deleted commits using `reflog`.
    
4. Diagnose issues with `git bisect`.
    
5. Clean repository: `git clean`, `git gc`.
    

**Exercises:**

- Delete a branch, then restore it using reflog.
    
- Find a bug introduced by a specific commit using bisect.
    

---

### ✅ Suggested Order

1. Chapter 1 → solidify basics
    
2. Chapter 2 → remote & collaboration
    
3. Chapter 3 → merge conflicts
    
4. Chapter 4 → rebasing & advanced branching
    
5. Chapter 5 → stashing
    
6. Chapter 6 → tags & releases
    
7. Chapter 7 → advanced log & history
    
8. Chapter 8 → aliases & productivity
    
9. Chapter 9 → real-world contribution workflow
    
10. Chapter 10 → recovery & debugging