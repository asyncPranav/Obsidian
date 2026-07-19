
---

# Git Roadmap — Absolute Zero to Open Source Contributor

> You know `add`, `commit`, `push`, `pull` — but not _why_ they work or what "remote," "origin," or "upstream" even mean. That's completely normal; most people learn git as a sequence of magic incantations first. This roadmap fixes that by explaining the _model_ first, then building real skills on top of it.

Every phase: **plain-English explanation → commands → a tiny hands-on exercise**. Do the exercises in a throwaway folder — don't worry about breaking anything, that's the point.

---

## Phase 0 — What even is git, really?

Before any commands, the mental model:

- Git is a program that lives **on your computer** and tracks changes to a folder over time.
- **GitHub is NOT git.** GitHub is a website that _hosts copies of your git folders online_ so you can back them up and share them. You could use git forever without ever touching GitHub.
- A **repository** ("repo") is just a folder that git is tracking. It has a hidden `.git` folder inside it where all the history lives.
- A **commit** is a saved snapshot of your folder at a point in time — like a save point in a video game. You choose when to create one (unlike autosave).

Try this:

```bash
mkdir git-practice && cd git-practice
git init          # turns this folder into a git repo — creates the hidden .git folder
ls -la             # you'll see a .git folder now exists
```

That `.git` folder is 100% local to your computer right now. Nothing has touched the internet yet.

---

## Phase 1 — The three areas (this unlocks everything else)

This is the single most important mental model in git. Once this clicks, every command makes sense.

Your project exists in **three places** at once:

```
1. Working Directory   →   2. Staging Area   →   3. Repository (commit history)
   (your actual files)      (add) puts files       (commit) saves
                             here, ready to           the staged
                             be committed             snapshot
```

- **Working directory** = the actual files you see and edit in your editor right now.
- **Staging area** (also called "the index") = a waiting room. When you run `git add`, you're not saving anything permanently yet — you're just saying "include this file in my _next_ snapshot."
- **Repository** = the permanent history. `git commit` takes whatever's in the staging area and locks it in as a snapshot forever (well, until you deliberately undo it — Phase 4).

Why does staging exist at all? So you can choose _exactly_ what goes into each commit, even if you've changed 5 files but only want 2 of them in this particular snapshot.

**Commands to practice**

```bash
echo "hello" > file1.txt
git status          # shows file1.txt as "untracked" — working dir has it, nothing else does

git add file1.txt
git status          # now shows it as "staged" — in the waiting room

git commit -m "add file1"
git status          # "nothing to commit, working tree clean" — it's now in history
```

**Mini exercise** Create 2 files. Stage only 1 of them. Run `git status` after each step and read the output carefully — git is very literal about telling you exactly which of the three areas each file is in.

---

## Phase 2 — What "remote" actually means

Now we connect to the internet — but slowly, one word at a time.

- A **remote** is just a nickname git uses for a _URL of another copy of this repo_, usually hosted on GitHub. Nothing more mystical than that.
- **`origin`** is not a special git keyword — it's just the _default name_ git gives to the first remote you connect, by convention. You could rename it "banana" and it would work identically. Everyone calls it `origin` because that's the default, so tutorials use it.
- When you `git clone <url>`, git automatically:
    1. Downloads the whole repo to your computer
    2. Automatically creates a remote called `origin` pointing back to that URL

So "pushing to origin" just means "uploading my local commits to that specific URL I have a nickname for."

**Commands to practice**

```bash
git remote -v                    # lists all remotes and their URLs (empty right now, we made this repo with `init`, not `clone`)

# Let's add one manually to see it in action (use any repo URL, even one of yours)
git remote add origin https://github.com/asyncpranav/some-repo.git
git remote -v                    # now shows origin pointing to that URL
```

**Mini exercise** Run `git remote -v` inside `checkmygit` (your existing clone) right now. You'll see `origin` pointing to _your fork_. This is proof that `git clone` set it up automatically for you without you ever typing `remote add`.

---

## Phase 3 — What `push` and `pull` are actually doing

Now that you know what a remote is, these commands stop being magic:

- **`git push <remote> <branch>`** = "take my local commits and upload them to that remote's version of this branch."
- **`git pull <remote> <branch>`** = "download any new commits from that remote and merge them into my local branch." (It's actually two commands combined: `git fetch` — download — then `git merge` — combine. More on this in Phase 6.)

You've been running `git push` and `git pull` without arguments — that works because git remembers a default remote+branch pairing once you've pushed once. But under the hood it's always "push _to_ a specific remote's specific branch."

**Commands to practice**

```bash
git push origin main             # explicit version of what "git push" usually does for you
git pull origin main             # explicit version of "git pull"
git fetch origin                 # downloads new commits but does NOT merge them in — just look, don't touch
git log origin/main              # after fetching, you can inspect what's new on the remote before pulling it in
```

**Mini exercise** In any repo, run `git fetch origin` then `git log HEAD..origin/main --oneline`. This shows commits that exist on the remote but not in your local branch yet — i.e., what a `pull` would bring in, _before_ you actually merge it. This habit (look before you merge) becomes important later.

---

## Phase 4 — Undoing mistakes (the skill that removes all fear)

You will mess up commits. Everyone does, constantly. Knowing you can always recover is what makes people comfortable experimenting with git instead of being afraid of it.

- `git restore <file>` = throw away uncommitted changes to a file, back to how it was at the last commit
- `git reset` = move your branch pointer backward, "undoing" commits (with options for what happens to the changes)
- `git revert` = create a _new_ commit that undoes an old one — safe to use even after pushing, since it doesn't rewrite history
- `git reflog` = a log of literally everywhere your `HEAD` has pointed, ever. Your ultimate safety net — almost nothing in git is permanently unrecoverable if you know to check here.

**Commands to practice**

```bash
git restore <file>               # discard uncommitted edits to one file

git reset --soft HEAD~1          # undo last commit, but keep the changes staged
git reset --mixed HEAD~1         # undo last commit, changes go back to working dir (unstaged)
git reset --hard HEAD~1          # undo last commit AND discard the changes entirely (careful!)

git revert <commit-hash>         # make a new commit that reverses an old one

git reflog                       # see history of HEAD movements — your undo-the-undo button
```

**Mini exercise (do this one, it's the most valuable exercise in this whole roadmap)**

1. Make a commit.
2. `git reset --hard HEAD~1` to make it "disappear."
3. Panic slightly (this is normal the first time).
4. Run `git reflog`, find the commit hash from before your reset.
5. Run `git reset --hard <that-hash>` to bring it back.

Once you've done this once, you'll know that git almost never actually deletes anything — it just moves pointers around. This single exercise is what turns git anxiety into git confidence.

---

## Phase 5 — Branches (what they actually are)

- A **branch** is nothing but a movable label pointing at one specific commit. That's the entire concept — people overcomplicate this.
- `main` is just a branch like any other, conventionally treated as "the real version."
- When you `git checkout -b my-branch`, you're saying "create a new label called `my-branch` pointing at the same commit I'm on right now, and start adding new commits under that label instead of `main`."
- This is why branching is fast and cheap in git — you're not copying files, just creating a new pointer.

**Commands to practice**

```bash
git branch                       # list local branches, * shows which you're on
git checkout -b feature/test      # create + switch to new branch in one step
git switch feature/test           # newer command, same idea as checkout for switching
git branch -d feature/test        # delete a branch (safe: refuses if unmerged changes exist)
git log --oneline --graph --all   # SEE the branches as pointers on a graph — do this often, it makes branches click visually
```

**Mini exercise** Create a branch, make one commit on it, switch back to `main`, make a _different_ commit on `main`. Now run `git log --oneline --graph --all` — you'll visually see two separate lines diverging from a shared point. That divergence is exactly what a "branch" is.

---

## Phase 6 — Forks and `upstream` (the open-source-specific piece)

Now the part that confused you most, explained slowly:

- A **fork** is a copy of someone else's repo, made _on GitHub_ (not on your computer), that lives under _your_ GitHub account. You can't push directly to someone else's repo — you don't have permission — so you fork it to get your own copy you _do_ have permission to push to.
- When you `git clone` **your fork**, git sets up `origin` = your fork's URL (as you learned in Phase 2).
- But now there's a second copy floating around: the _original_ repo (the one you forked from). Git doesn't know about it automatically — you have to add it yourself as a second remote, and by convention everyone names this one **`upstream`**.

So the full picture for open source:

```
Original repo (whoisyurii/checkmygit)  →  called "upstream" once you add it as a remote
        ↓ (you clicked Fork)
Your copy (asyncpranav/checkmygit)     →  called "origin" — this is what you actually cloned
        ↓ (git clone)
Your computer's local copy              →  where you branch, commit, and edit
```

Why do you need `upstream` at all? Because the original repo keeps getting new commits from other contributors while you're working. Without `upstream`, your fork slowly goes stale, and you'd be trying to merge your changes into old, outdated code.

**Commands to practice**

```bash
git remote add upstream https://github.com/whoisyurii/checkmygit.git
git remote -v                    # now shows BOTH origin (your fork) and upstream (the real repo)

git fetch upstream                # download the real repo's latest commits (doesn't touch your files yet)
git checkout main
git merge upstream/main           # bring those new commits into your local main
git push origin main              # push the updated main to YOUR fork too, so it's not stale online either
```

**Mini exercise** In your `checkmygit` clone, run `git remote -v` right now and read the output slowly — you should be able to point at each line and say out loud what it is and why it's there, using the diagram above.

---

## Phase 7 — Keeping your feature branch up to date

Once `upstream` is set up, before opening any PR, sync your feature branch too — not just `main`:

```bash
git checkout fix/my-branch
git fetch upstream
git rebase upstream/main          # replay YOUR commits on top of the latest upstream code
git push --force-with-lease origin fix/my-branch   # push the rebased branch back to your fork
```

`--force-with-lease` (never plain `--force`) is needed here because rebasing rewrites your branch's commit history — it's a safe version of force-push that fails if someone else's work would be overwritten.

**Concept: merge vs rebase**

- `merge` combines two branches and adds a new "merge commit" — history stays exactly as it happened, including all the messy back-and-forth.
- `rebase` takes your commits and replays them _on top of_ the other branch, as if you'd started from there — cleaner, linear history. This is what most OSS maintainers prefer to see in a PR.

---

## Phase 8 — Resolving conflicts without panicking

A conflict happens when git can't automatically combine two changes because they touched the same lines differently.

When it happens, git pauses and marks the file like this:

```
<<<<<<< HEAD
your version of the line
=======
their version of the line
>>>>>>> upstream/main
```

You manually edit the file to keep whichever version (or a combination) is correct, delete the `<<<<<<<`, `=======`, `>>>>>>>` markers, then:

```bash
git add <the-file-you-fixed>
git rebase --continue        # if you were rebasing
# or
git commit                   # if you were merging

git rebase --abort           # bail out completely and go back to before you started, if it's too messy
```

**Mini exercise** Deliberately cause a conflict: on `main` change line 1 of a file to "AAA" and commit. Create a branch from _before_ that commit, change line 1 to "BBB" and commit. Merge the branch into `main`. Resolve the conflict by hand. Do this once in a throwaway repo so real conflicts later feel familiar instead of scary.

---

## Phase 9 — Writing good commits (what makes maintainers trust your PRs)

- **Conventional commit style**: `fix: ...`, `feat: ...`, `docs: ...`, `refactor: ...`, `chore: ...` — a one-word prefix telling reviewers the _type_ of change at a glance.
- **Atomic commits**: one logical change per commit. Not "fixed bug and also reformatted 10 files."
- **Good message structure**: short summary line (under ~50 characters), blank line, then a body explaining _why_ the change was needed — the diff itself already shows _what_ changed.

```bash
git commit --amend               # fix your last commit's message, or add a forgotten file to it, before pushing
git add -p                       # stage a file's changes in chunks — lets you split one messy edit session into multiple clean, atomic commits
```

**Mini exercise** Edit two unrelated parts of one file in a single sitting. Use `git add -p` to split them into two separate, properly-labeled commits instead of dumping both into one.

---

## Phase 10 — Investigating history like a debugger

```bash
git blame <file>                 # see who last changed each line, and in which commit — great for understanding *why* code is written a certain way
git log --oneline --graph --all  # visualize the whole commit tree
git log --author="name"
git log --grep="keyword"

git bisect start                 # binary-search through history to find which commit introduced a bug
git bisect bad                   # mark current commit as broken
git bisect good <old-hash>       # mark a known-working commit
# git checks out commits for you to test; mark each good/bad until it finds the exact culprit
git bisect reset
```

---

## Phase 11 — Small extra tools you'll bump into

```bash
git stash                        # temporarily shelve uncommitted changes (e.g. you need to switch branches urgently)
git stash pop                    # bring them back

git cherry-pick <commit-hash>    # copy one specific commit from another branch onto your current one

git tag v1.0.0                   # label a specific commit as a release point
```

---

## Phase 12 — The actual open-source contribution checklist

This is where everything above becomes real practice, using what you just did on checkmygit as the template:

1. Fork the repo on GitHub (creates your copy)
2. `git clone` your fork (sets up `origin` automatically)
3. `git remote add upstream <original-repo-url>` (so you can stay in sync)
4. Read `CONTRIBUTING.md` if the repo has one
5. Check open issues/PRs so you don't duplicate work
6. `git checkout -b fix/clear-branch-name`
7. Make your change — keep the diff minimal and focused
8. Test it locally
9. Commit with a clear `type: message`
10. `git fetch upstream && git rebase upstream/main` — make sure you're on top of the latest code
11. `git push origin <branch>` (use `--force-with-lease` if you rebased after already pushing once)
12. Open the PR: **what was wrong → why it happened → what you changed → how you tested it**
13. Respond to review comments with new commits on the same branch

---

## Suggested pace

|Week|Focus|
|---|---|
|1|Phase 0–3 — the mental model: areas, remotes, push/pull explained|
|2|Phase 4–5 — undoing mistakes + branches|
|3|Phase 6–7 — forks, upstream, syncing (directly useful for your next OSS PR)|
|4|Phase 8–9 — conflicts + commit hygiene|
|5|Phase 10–12 — investigation tools + the full contribution checklist|

You don't need to finish this before contributing again — you already proved that on checkmygit. Use it as a reference to revisit after every PR, checking off what's become automatic.

---

## Cheat sheet — glossary in one place

|Term|Plain-English meaning|
|---|---|
|Working directory|The actual files you see/edit|
|Staging area|The "waiting room" before a commit — `git add` puts things here|
|Repository|Permanent saved history — `git commit` locks a snapshot in|
|Remote|A nickname for a URL of another copy of the repo, usually on GitHub|
|`origin`|The conventional name for the first/main remote (usually your fork or your own repo)|
|`upstream`|The conventional name for _the original_ repo you forked from|
|Branch|A movable label pointing at a commit|
|`HEAD`|A pointer to whichever commit/branch you currently have checked out|
|Merge|Combine two branches, keeping full history including a merge commit|
|Rebase|Replay your commits on top of another branch for clean, linear history|
|Conflict|Git can't auto-combine two changes to the same lines — you resolve manually|
|Reflog|Full history of every place `HEAD` has pointed — your ultimate undo button|
