## DevOps Day-27: Git Revert Some Changes (Storage Server)

### 🔹 Task Goal
Revert the **latest commit (HEAD)** in the Git repository so that the repository returns to the **previous commit** state.
* Repository: `/usr/src/kodekloudrepos/ecommerce`
* Action: Revert **HEAD**
* Commit message: **`revert ecommerce message`** (all lowercase)
⚠️ Important:
* Use **git revert**, not reset
* This creates a **new commit** that undoes changes (safe for shared repos)

### 🔹 Login to Storage Server
ssh natasha@ststor01                  # Login to storage server

### 🔹 Switch to Root User (If Required)
sudo -i                              # Switch to root (repo owned by root)

### 🔹 Navigate to Git Repository
cd /usr/src/kodekloudrepos/ecommerce  # Enter ecommerce repository
git status                           # Check current branch and working tree

### 🔹 Revert the Latest Commit (HEAD)
git revert HEAD -n                   # Revert latest commit without auto-commit
# HEAD → latest commit
# -n / --no-commit → apply revert changes but do not commit yet
git status                           # Verify reverted changes are staged/unstaged

### 🔹 Stage Reverted Changes
git add .                            # Stage all reverted changes
git status                           # Confirm changes are staged

### 🔹 Commit the Revert
git commit -m "revert ecommerce message"  # Create revert commit

✔ This new commit safely undoes the previous commit
---

### 📝 Summary
* Identified latest commit using HEAD
* Used **git revert** to undo changes safely
* Created a new commit pointing repo back to previous state
* Maintained clean and auditable Git history
---
### 🎯 Interview Tips
* **git revert** → Safe for shared repositories
* **git reset** → Rewrites history (dangerous on shared repos)
* **HEAD** → Refers to the latest commit
* **-n / --no-commit** → Allows custom commit message

✔ Common DevOps incident-fix scenario
