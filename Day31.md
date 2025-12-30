# DevOps Day-31 – Git Stash (Restore, Commit & Push)

Task Goal:
• Find stashed changes
• Restore stash@{1}
• Commit restored changes
• Push to origin

# What is git stash?
Git stash is a Git feature that temporarily saves your uncommitted changes (modified + staged files) and cleans your working directory, so you can switch tasks or branches without committing unfinished work.
Think of it as a temporary shelf for your work 🧳.

# Why do we use git stash?
We use git stash when we:
Are in the middle of work
Are not ready to commit
Need to switch branch, pull updates, or fix something urgent
Want a clean working directory
---

cd /usr/src/kodekloudrepos/games
# Navigate to the git repository

ls
# List files in repository

git status
# Check current working tree state

git stash list
# Display all saved stashes
# Example output:
# stash@{0}: WIP on master
# stash@{1}: WIP on master

git stash apply stash@{1}
# Apply changes from stash@{1}
# Stash remains in stash list

git status
# Verify restored files are now modified

cat welcome.txt
# View content of restored file

git commit -am "apply stash@{1}"
# Commit restored changes
# -a stages tracked files automatically

git push
# Push committed changes to origin repository

history
# Show command execution history
----------------------------------------------------------------------------------------------------------------

# Git Stash Practice Example (new.txt)
Create & Stash Changes ;
vi new.txt
# Create or edit file

git status
# Shows new.txt as untracked

git add new.txt
# Stage the file

git status
# File is staged

git stash save "temp save"
# Stash staged + unstaged changes
# Working tree becomes clean

git status
# Confirm clean working directory

ls
# new.txt no longer visible

Restore Stash
git stash list
# View available stashes

git stash pop stash@{0}
# Apply stash and REMOVE it from stash list

git stash list
# Confirm stash is removed

git status
# Verify restored file status

cat new.txt
# Check restored file content

Cleanup (Optional)
git restore --staged new.txt
# Unstage file

git status
# new.txt now unstaged

rm new.txt
# Remove file

ls
# Confirm file deletion

Key Git Stash Commands (Exam Ready)
git stash save "msg"   → Save work temporarily
git stash list         → Show all stashes
git stash apply        → Apply stash (keep stash)
git stash pop          → Apply + delete stash

Important Notes ⚠️
apply → stash remains
pop   → stash removed
stash works only on tracked files
-----------------------------------------------------------------------------------------------------------------

# Simple Example (Real Life)
You are editing files, and suddenly your team says:
“Switch branch and fix a production bug NOW!”
But Git says:
error: Your local changes would be overwritten
👉 Solution: stash the changes
---
git stash

Now your working directory is clean, and you can switch branches safely.
What exactly does git stash save?
By default, it saves:
Modified tracked files
Staged files
❌ It does NOT save:
Untracked files (unless -u is used)
Basic Git Stash Commands (Must Know)

git stash
# Save changes and clean working directory

git stash list
# View all saved stashes

git stash apply
# Restore stash (stash remains)

git stash pop
# Restore stash and delete it

git stash drop stash@{0}
# Delete a specific stash

# When should you use git stash?

✅ Use git stash when:
Work is temporary
Changes are not complete
You’ll come back to it soon
❌ Do NOT use git stash when:
Work is complete → use git commit
Long-term storage → stash can be forgotten

Git Stash vs Git Commit ;

| Feature         | git stash | git commit |
| --------------- | --------- | ---------- |
| Temporary       | ✅ Yes     | ❌ No       |
| Saves history   | ❌ No      | ✅ Yes      |
| Safe long-term  | ❌ No      | ✅ Yes      |
| Switch branches | ✅ Yes     | ✅ Yes      |

Git stash temporarily saves uncommitted changes so we can work on something else without committing unfinished work.