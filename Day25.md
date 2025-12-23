## DevOps Day-25: Git Merge Branches (Storage Server)

### 🔹 Task Goal
Create a feature branch, add a file, commit changes, merge the branch into **master**, and push changes to the remote repository.
Repository path:
`/usr/src/kodekloudrepos/demo`

### 🔹 Login to Storage Server
ssh natasha@ststor01                  # Login to storage server

### 🔹 Verify Repository Path
ls -lah /usr/src/kodekloudrepos/demo/ # Check demo repository exists

### 🔹 Switch to Root User (If Required)
sudo -i                              # Become root (repo owned by root)
cd /usr/src/kodekloudrepos/demo      # Move into demo Git repository
git status                           # Check current branch & working tree

### 🔹 Check Existing Branches
git branch                           # List local branches

### 🔹 Create and Switch to New Branch
git checkout -b nautilus             # Create and switch to 'nautilus' branch
# -b → create new branch
# nautilus → feature branch name
git branch                           # Confirm branch switch (* indicates active)

### 🔹 Add New File to Branch
ls /tmp/index.html                   # Verify source file exists
cp /tmp/index.html .                 # Copy file into repo
ls                                  # Confirm file copied
git status                           # Git detects new untracked file

### 🔹 Stage and Commit Changes
git add index.html                   # Stage the new file
git status                           # Verify file is staged
git commit -m "added index.html file"  # Commit changes to nautilus branch

### 🔹 Switch Back to Master Branch
git checkout master                  # Move to master branch
git branch                           # Confirm active branch is master
git log --oneline                    # View commit history before merge

### 🔹 Merge Feature Branch into Master
git merge nautilus                   # Merge nautilus branch into master
git log --oneline                    # Verify merge commit appears
ls                                  # Confirm index.html exists in master

### 🔹 Push Changes to Remote Repository
git push                             # Push merged master branch to origin

### 🔹 Push Feature Branch (Optional)
git checkout nautilus                # Switch back to feature branch
git push                             # May fail if branch not set upstream
git push -u origin nautilus          # Push branch and set upstream
# -u → sets upstream tracking branch

### 📝 Summary
* Created feature branch `nautilus`
* Added and committed `index.html`
* Merged feature branch into `master`
* Pushed merged code to remote repository
* Set upstream for feature branch
---
### 🎯 Interview Tips
* **git checkout -b** → create + switch branch
* **git merge** → integrates feature branch into target branch
* **git push -u** → required for first-time branch push
* Always merge into **master/main** from a clean working tree

✔ Common DevOps Git workflow
