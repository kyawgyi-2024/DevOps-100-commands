# DevOps Day-33 – Resolving Git Merge Conflicts (Rebase Method)

Scenario Summary
• Sarah and Max both pushed changes to master
• Remote master has newer commits
• Max’s local master cannot push (non-fast-forward)
• Use git pull --rebase to avoid merge commits
• Resolve conflict in story-index.txt

1️⃣ Login & Navigate to Repository
ssh max@ststor01
# Login as user max

cd ~/story-blog
# Go to the story-blog repository

ls
# Verify story files exist

2️⃣ Check Branch Status
git status
# Local master is ahead by 1 commit
# Working tree is clean

3️⃣ Fix Required Content (Before Push)
vi story-index.txt
# Ensure ALL 4 story titles are present
# Fix typo: Mooose → Mouse
Correct content:
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog

4️⃣ Commit the Fix
git add story-index.txt
# Stage corrected file

git commit -m "fix type error"
# Commit changes

git commit --amend
# Correct commit message to: "fix typo error"

5️⃣ Push Attempt (Fails)
git push
# Push rejected
# Remote master contains new commits
📌 Reason: Someone (Sarah) already pushed changes.

6️⃣ Rebase Local Changes on Remote Master
git pull --rebase
# Reapply local commits on top of origin/master
Conflict Occurs
CONFLICT (add/add): story-index.txt
👉 Both users modified the same file.

7️⃣ Identify Conflict State
git status
# Rebase in progress
# story-index.txt marked as unmerged

8️⃣ Resolve Merge Conflict
vi story-index.txt
# Remove conflict markers
# Keep all 4 story titles
# Fix typo (Mouse)

git add story-index.txt
# Mark conflict as resolved

9️⃣ Continue Rebase
git rebase --continue
# Complete rebase after conflict resolution

git status
# Branch clean, rebase successful

🔟 Push Final Changes
git push
# Push succeeds
# Remote master updated

Final Outcome ✅
✔ All 4 stories listed
✔ Typo corrected (Mooose → Mouse)
✔ Merge conflict resolved
✔ Rebase completed
✔ Changes pushed successfully

Key Commands (Exam-Ready)
git pull --rebase        # Sync without merge commit
git status              # Check conflict state
git add <file>          # Mark conflict resolved
git rebase --continue   # Finish rebase
git push                # Upload changes

Important Concepts ⭐
• Push fails → remote has newer commits
• Rebase avoids merge commits
• Conflicts must be resolved manually
• git add marks conflict resolved

One-Line Interview Answer

Git merge conflicts happen when multiple changes affect the same file, and they are resolved by manually fixing the file, staging it, and completing the rebase or merge.