---
title: Git cheat sheet
date: 2025-12-13
categories: [GIT]
tags: [git, quality-of-life]     # TAG names should always be lowercase
---

# GENERAL QUALITY OF LIFE IMPROVEMENTS #1

## Git cheat sheet

## Essential Git History Cleanup & Integration Commands

These commands are used to modify commit history, integrate features without merging noise, and update remote branches.

## 1. Integrate a Feature Branch as a Single Commit (Squash Merge)

This is ideal for integrating an entire feature branch into your current branch as one clean commit, eliminating the individual commits from the feature's history.

### Step 1: Fetch the branch
Downloads the branch contents from the remote to ensure you have the latest changes.

```zsh
git fetch origin feat/feature-branch
```

### Step 2: Apply changes
Applies all changes from the feature branch to your working directory without creating a commit yet.

```zsh
git merge --squash feat/feature-branch
```

### Step 3: Final Commit
Commits all the staged changes as a single, clean commit.

```zsh
git commit -m "feat: feature message"
```

---

## 2. Amend the Latest Commit Message

Use this to fix a typo or add missing detail to the most recent commit you created.

```zsh
# Amend the commit and change the message
git commit --amend -m "fix: new commit message"

# Force push is required because you changed history
git push -f
```

---

## 3. Squash ALL Commits into One (Quick Reset)

This is a fast way to wipe the history of your current branch and replace it with a single "root" commit. **Use with caution, as it completely rewrites history.**

```zsh
# Resets the branch to the initial state of the current code.
# The resulting commit will only contain the code changes.
git reset $(git commit-tree HEAD^{tree} -m "development")

# Force push is required to overwrite the remote history
git push -f
```

---

## 4. Squash a Specific Number of Commits (e.g., 5 commits)

This allows you to combine the last $N$ commits into a single commit.

```zsh
# Soft reset back 5 commits. Changes are kept in the staging area.
git reset --soft HEAD~5

# Create a new commit containing all the staged changes (from the 5 squashed commits)
git commit -m "message"

# Force push the rewritten history
git push -f
```

---

## 5. Bring `dev` Changes into a Feature Branch (Without Extra Merge Commit)

This is the standard, clean way to pull recent changes from a base branch (`dev`) into your feature branch.

*(You must be on your feature branch, and the `dev` branch must be up-to-date locally.)*

```zsh
# Rewrites your feature branch's history to start *after* the latest 'dev' commit.
# This keeps the history linear and avoids unnecessary merge commits.
git rebase dev

# Force push is required because rebasing rewrites the branch history
git push -f
```
