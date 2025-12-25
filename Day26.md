## DevOps Day-26: Git Manage Remotes (Storage Server)

### 🔹 Task Goal
Add a **new Git remote**, commit changes, and push the **master** branch to the new remote repository.

Repository path:
`/usr/src/kodekloudrepos/apps`

### 🔹 Login to Storage Server
ssh natasha@ststor01                  # Login to storage server
### 🔹 Switch to Root User (If Required)
sudo -i                              # Switch to root (repo owned by root)

### 🔹 Navigate to Repository
cd /usr/src/kodekloudrepos/apps/      # Enter the apps Git repository

git remote -v                       # List existing remotes (fetch & push URLs)

### 🔹 Verify New Remote Repository Path
ls /opt/                             # Check available bare repositories

### 🔹 Add a New Git Remote
git remote add dev_apps /opt/xfusioncorp_app.git
# dev_apps → New remote name
# /opt/xfusioncorp_app.git → Bare repository destination
git remote -v                       # Verify new remote added successfully
### 🔹 Prepare File for Commit
ls /tmp/index.html                   # Verify source file exists
cat /tmp/index.html                  # View file content
cp /tmp/index.html .                 # Copy file into current repository
ls                                  # Confirm index.html copied

### 🔹 Stage and Commit Changes
git branch                           # Verify current branch (master)
git add index.html                   # Stage the new file
git status                           # Confirm file is staged
git commit -m "added index.html file"  # Commit changes

### 🔹 Push to New Remote
git push -u dev_apps master          # Push master branch to new remote
# -u → Set upstream tracking branch
# dev_apps → Remote name
# master → Branch name

### 🔹 Verify Commit History
git log --oneline                    # View commit history

### 📝 Summary

* Verified existing remotes
* Added new remote `dev_apps`
* Copied and committed `index.html`
* Pushed master branch to new remote repository
* Set upstream for future pushes

---
### 🎯 Interview Tips
* **git remote add** → Connects repo to another remote
* **Multiple remotes** are common in DevOps (origin, staging, prod)
* **git push -u** simplifies future pushes

✔ Essential Git skill for CI/CD pipelines
