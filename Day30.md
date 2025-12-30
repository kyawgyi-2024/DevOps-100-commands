# DevOps Day-30 : Git Reset Commit History (Keep Only 2 Commits)

Goal:
Reset the git repository so that only these two commits remain:
initial commit
add data.txt file
Then push the cleaned history to remote.
Repository Location : /usr/src/kodekloudrepos/games

# 1️⃣ Go to the repository
cd /usr/src/kodekloudrepos/games
👉 Moves into the git repository directory.

# 2️⃣ Check current repository status
git status

👉 Confirms:
Current branch
Working tree is clean
No uncommitted changes

# 3️⃣ View commit history (short format)
git log --oneline

👉 Shows commit history like:
33c3031 add data.txt file
ab12345 some test commit
...
📌 Identify the commit hash for add data.txt file

# 4️⃣ Reset branch and HEAD to required commit
git reset --hard 33c3031

👉 Most important command
What it does:
Moves HEAD to commit 33c3031
Deletes all commits after this commit
Cleans working tree & staging area
📌 Now history contains only:
initial commit
add data.txt file
⚠️ --hard means changes are permanently removed

# 5️⃣ Verify commit history again
git log --oneline

👉 Output should show only two commits:
33c3031 add data.txt file
xxxxxxx initial commit

# 6️⃣ Confirm clean working tree
git status

👉 Ensures:
nothing to commit, working tree clean

# 7️⃣ Push changes to remote repository
git push

👉 This may fail because history was rewritten.

# 8️⃣ Force push updated history (Required)
git push -f

👉 Force push is needed because:
Commit history was rewritten using git reset
Remote history must be overwritten
📌 Successfully updates the remote repository with cleaned history.

Final Result ✅
✔ Only 2 commits exist in repository
✔ HEAD and branch point to add data.txt file
✔ Remote repository updated
✔ Test commits removed permanently

# Key Commands Summary (Quick Revision)
git log --oneline        # View commit history
git reset --hard <id>   # Reset HEAD and delete later commits
git push -f             # Force push rewritten history

-------------------------------------------------------------------------------------------------------------

# DevOps Day-30 – Git Reset Commit History (One-Page CLI Sheet)

### 🔹 Login to Storage Server
ssh natasha@ststor01                 # Login to storage server
cd /usr/src/kodekloudrepos/games
# Move into the git repository directory

ls
# List repository files to confirm location

git status
# Check current branch and working tree status

git log --oneline
# View commit history in short format
# Identify commit with message: "add data.txt file"

git reset --hard 33c3031
# Reset HEAD and current branch to commit 33c3031
# Deletes all commits AFTER this commit
# Working tree and staging area cleaned

git log --oneline
# Verify only two commits exist:
# 1) initial commit
# 2) add data.txt file

git status
# Confirm working tree is clean

git push
# Attempt normal push (will fail due to rewritten history)

git push -f
# Force push updated commit history to remote repository
# Required after git reset --hard

history
# View executed command history

# Final State (Expected)
✔ Only 2 commits in repository
✔ HEAD points to "add data.txt file"
✔ All test commits removed
✔ Remote repository updated

# Key Reminder (Exam / Interview)
git reset --hard  → rewrites history (local)
git push -f       → overwrites remote history
Use carefully in shared repositories