## DevOps Day-28: Git Cherry-Pick (Storage Server)

### 🔹 Task Goal
Apply **one specific commit** from the `feature` branch into the `master` branch **without merging the whole branch**, then push the changes.
* Repository: `/usr/src/kodekloudrepos/media`
* Branches: `master`, `feature`
* Target commit message: **`Update info.txt`**

### 🔹 Login to Storage Server
ssh natasha@ststor01                 # Login to storage server

### 🔹 Switch to Root User (If Required)
sudo -i                             # Switch to root (repo owned by root)

### 🔹 Navigate to Repository
cd /usr/src/kodekloudrepos           # Move to repositories directory
ls                                  # List repositories
cd media/                           # Enter media repository
ls                                  # Verify repo contents
git status                          # Check working tree status

### 🔹 Verify Branches and Commit History
git branch                          # List available branches
git log --oneline                   # View commit history (note commit ID with message "Update info.txt")

### 🔹 Switch to Master Branch
git checkout master                 # Switch to master branch
git branch                          # Confirm active branch (* master)
git status                          # Ensure clean working tree
git log --oneline                   # View master branch commits

### 🔹 Cherry-Pick Required Commit
git cherry-pick <commit_id>         # Apply specific commit from feature branch
# <commit_id> → Commit hash of "Update info.txt"
git log --oneline                   # Verify commit added to master

### 🔹 Push Changes to Remote
git push                            # Push updated master branch

### 📝 Summary
* Identified required commit from `feature` branch
* Switched safely to `master` branch
* Used **git cherry-pick** to apply only one commit
* Avoided merging incomplete feature work
* Pushed changes to remote repository
---
### 🎯 Interview Tips
* **git cherry-pick** → Apply a specific commit only
* **git merge** → Merge entire branch history
* Used when **hotfixes or urgent changes** are needed
* Common in **DevOps production fixes**

✔ Safe, precise, and professional Git workflow
