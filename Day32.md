# DevOps Day-32 – Git Rebase (Feature → Master)

Goal
• Update feature branch with latest master changes
• Do NOT create a merge commit
• Do NOT lose feature branch work
• Push rebased feature branch to remote

Repository Details
Remote repo : /opt/official.git
Local path  : /usr/src/kodekloudrepos/official
Working br. : feature
Base branch : master
---
# Step-by-Step Commands (With Explanation)

cd /usr/src/kodekloudrepos
# Move to cloned repositories directory

cd official/
# Enter the project repository

git status
# Confirm current branch is 'feature'
# Working tree must be clean before rebase

git branch
# Verify available branches
# * feature
#   master

git remote -v
# Confirm remote repository location

# Fetch Latest Master Changes (No Merge)
git fetch origin master
# Download latest master branch commits from remote
# Does NOT modify local branches

git fetch -a
# Fetch all remote branches and updates

# Rebase Feature Branch on Master
git rebase origin/master
# Replay feature commits on top of latest master
# No merge commit created
# Feature history rewritten cleanly

git log --oneline
# Verify linear commit history
# Feature commit appears on top of master commits
Example:
29b3b15 (HEAD -> feature) Add new feature
1c09086 (origin/master, master) Update info.txt
eb08aa0 initial commit

# Push Rebased Feature Branch
git push
# Fails because feature branch has no upstream

git push -u origin feature
# Sets upstream but fails due to rewritten history
# Non-fast-forward error expected after rebase

git push -u origin feature -f
# Force push required after rebase
# Updates remote feature branch with new history

Final Result ✅
✔ Feature branch rebased on master
✔ No merge commit created
✔ Feature work preserved
✔ Remote feature branch updated
✔ Clean, linear commit history

# Key Rebase Concepts (Exam Ready)
git rebase origin/master
→ Replays feature commits onto master

git fetch
→ Gets updates without merging

git push -f
→ Required after rebase (history rewritten)

# Golden Rule ⚠️
Never rebase a shared branch unless you force push

# One-Line Interview Answer ⭐
Git rebase is used to replay feature branch commits on top of the latest master branch to maintain a clean, linear history without creating merge commits.

==============================================================================================

# DevOps-Day-32: Git Rebase, exactly matching what you did and why each command is used.

🧠 Scenario Understanding (Before Commands)
Repository path:
Remote: /opt/official.git
Local clone: /usr/src/kodekloudrepos/official

Current situation:
Developer is working on feature branch
master branch has new commits

Requirement:
✅ Update feature branch with latest master changes
✅ No merge commit allowed
✅ Do not lose feature branch work
✅ Push changes after rebase

# 👉 Correct solution: git rebase

# 📂 Step 1: Move into Repository
cd /usr/src/kodekloudrepos/official
✔️ Navigates into the local Git repository.

# 🌿 Step 2: Confirm Current Branch
git status
---
Output: On branch feature
nothing to commit, working tree clean
✔️ Confirms:
You are on feature branch
No uncommitted changes (safe to rebase)

# 🌿 Step 3: List Branches
git branch
---
Output:
* feature
  master
✔️ Confirms:
Feature branch is checked out
Master branch exists locally

# 🌐 Step 4: Verify Remote Repository
git remote -v
---
Output:
origin  /opt/official.git (fetch)
origin  /opt/official.git (push)
✔️ Confirms remote repo is properly configured.

# 🔄 Step 5: Fetch Latest Master Branch
git fetch origin master
---
✔️ Downloads latest commits from master without merging
✔️ Safe operation (no code changes yet)

# 🔁 Step 6: Rebase Feature Branch on Master
git rebase origin/master
---
What this does:
Takes feature branch commits
Re-applies them on top of latest master
Keeps history linear
❌ No merge commit created
Output:
Successfully rebased and updated refs/heads/feature.
✅ Requirement satisfied

# 📜 Step 7: Verify Commit History
git log --oneline
---
Output:
29b3b15 (HEAD -> feature) Add new feature
1c09086 (origin/master, master) Update info.txt
eb08aa0 initial commit
✔️ Confirms:
Feature commit is now after master commit
Rebase successful

# 🚀 Step 8: Push Feature Branch (Why Force Is Needed)
Normal push attempt:
---
git push -u origin feature
---
❌ Fails because:
Rebase rewrites commit history
Remote branch still has old commit hash

# ⚠️ Step 9: Force Push (Correct & Required)
git push -u origin feature -f
---
Why -f (force) is required:
Rebase creates new commit hashes
Git sees history mismatch
Force push updates remote feature branch
Output:
feature -> feature (forced update)
branch 'feature' set up to track 'origin/feature'
✔️ Push successful
✔️ Upstream branch set
✅ Final Outcome
✔️ Feature branch rebased with latest master
✔️ No merge commit added
✔️ No feature work lost
✔️ Changes pushed successfully

📌 Important Interview Notes
TopicExplanationWhy rebase?Keeps history clean and linearWhy not merge?Merge creates extra commitWhy force push?Rebase rewrites commit historySafe to rebase?Yes, if branch is private

🧠 One-Line Summary
Rebase replays feature commits on top of master, and force push updates rewritten history safely.
---------------------------------------------------------------------------------------------------
