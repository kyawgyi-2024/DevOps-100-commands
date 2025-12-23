## DevOps Day-24: Create Git Branch (Storage Server)

### 🔹 Task Goal
Create a new Git branch **`xfusioncorp_apps`** from the **`master`** branch in the repository:
`/usr/src/kodekloudrepos/apps`

### 🔹 Login to Storage Server
ssh natasha@ststor01                 # Login to storage server as user 'natasha'

### 🔹 Navigate to Repository
cd /usr/src/kodekloudrepos           # Move to repositories directory
ls                                  # List available repositories
cd apps/                            # Enter the apps Git repository
git status                           # Check current branch and working tree state

### 🔹 Switch to Root User (If Required)
sudo -i                             # Switch to root user (needed if repo owned by root)
cd /usr/src/kodekloudrepos/apps/     # Ensure correct repository path
git status                           # Confirm clean working tree
# Output example:
# On branch kodekloud-apps
# nothing to commit, working tree clean

### 🔹 View Existing Branches
git branch                           # List all local branches

### 🔹 Switch to Master Branch
git checkout master                  # Switch to master branch
git branch                           # Verify current branch (* indicates active branch)

### 🔹 Create New Branch from Master
git checkout -b xfusioncorp_apps     # Create and switch to new branch
# -b → Create a new branch
# xfusioncorp_apps → New branch name
git status                           # Verify current branch is xfusioncorp_apps
git branch                           # List branches and confirm new branch exists

### 📝 Summary
* Repository located at `/usr/src/kodekloudrepos/apps`
* Verified clean working tree before branching
* Switched to `master` branch
* Created new branch `xfusioncorp_apps` from `master`
* Successfully switched to the new branch
✔ Branch ready for feature development or customization

### 🎯 Interview Tip
> **Creating branches from master ensures new features or custom changes do not affect stable production code.**

✔ Common DevOps and Git workflow practice
