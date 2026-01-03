# Day 34: Git Hook | 100 Days of DevOps

Repository: /opt/games.git (this is the bare repo)
Working clone: /usr/src/kodekloudrepos
Merge feature → master
Create a post-update hook in /opt/games.git/hooks/
When master is pushed, automatically create a tag:
release-YYYY-MM-DD
Test it (tag must exist)

Push changes
❌ Do NOT change permissions/ownership of repo directories

✅ STEP 1: Go to the working repository
---
cd /usr/src/kodekloudrepos

Check branches:
---
git branch

✅ STEP 2: Merge feature → master
---
git checkout master
git merge feature
👉 Resolve conflicts if any (usually none in exam)

✅ STEP 3: Create post-update hook (IMPORTANT)
Go to the bare repo hooks directory:
---
cd /opt/games.git/hooks

Create hook file:
---
vi post-update

🔹 Paste THIS (POSIX-safe, exam-friendly)
#!/bin/sh

for refname in "$@"
do
    if [ "$refname" = "refs/heads/master" ]; then
        TODAY=$(date +%F)
        git tag -a -f "release-$TODAY" -m "release for $TODAY"
    fi
done
---
Save and exit.

✅ STEP 4: Make hook executable (ONLY the hook)
⚠️ This does NOT alter repo permissions — it’s allowed.
---
chmod +x /opt/games.git/hooks/post-update

✅ STEP 5: Push master (this TRIGGERS the hook)
Go back to working repo:
---
cd /usr/src/kodekloudrepos
git push origin master

You should NOT see errors.
✅ STEP 6: Verify tag was created (SERVER SIDE)
---
cd /opt/games.git
git tag

Expected output (example):
release-2023-06-15


✅ This proves the hook worked.
(Optional but good) Verify from client
---
cd /usr/src/kodekloudrepos
git fetch --tags
git tag

🧠 Why this works (important for exams)
Item	Reason
Bare repo	Hooks must be here
post-update	Receives pushed refs
refs/heads/master	Only tag on master
date +%F	Correct YYYY-MM-DD
No permission changes	Exam safe
🚨 Common mistakes (you avoided)
❌ Using [[ ]]
❌ Creating hook in working repo
❌ Forgetting chmod +x
❌ Expecting tag to appear locally without fetch --tags

✅ FINAL CHECKLIST (exam-style)
✔ Feature merged into master
✔ Hook created in /opt/games.git/hooks/post-update
✔ Hook executable
✔ Push tested
✔ Release tag created
✔ No permission changes

===============================================================================================

The Nautilus application development team was working on a git repository /opt/official.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. The team want to setup a hook on this repository, please find below more details:

Merge the feature branch into the master branch, but before pushing your changes complete below point.

Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release.

Finally remember to push your changes.
Note: Perform this task using the natasha user, and ensure the repository or existing directory permissions are not altered. (rewrite and explain details) 

Sure 👍
Below is a clean rewrite of the task plus a clear explanation, written in a way that’s easy to understand and perfect for Day 34 – Git Hooks (100 Days of DevOps / KodeKloud style).

🔹 Rewritten Task Description (Simple & Clear)

The Nautilus application development team is working with a Git repository located at /opt/official.git.
This repository is a bare repository and has already been cloned into the directory /usr/src/kodekloudrepos on the Storage server in the Stratos Data Center.

The team wants to automate the release process using a Git hook.

Before pushing any changes, you must merge the feature branch into the master branch.

After that, configure a post-update Git hook in the repository so that whenever changes are pushed to the master branch, a release tag is automatically created.
The tag must follow this naming format:

release-YYYY-MM-DD


Here, YYYY-MM-DD represents the current date.
For example:

If today is 15 June 2023, the tag should be: release-2023-06-15

If today is 20 June 2023, the tag should be: release-2023-06-20

Once the hook is created, you must test it at least once by pushing changes to the master branch and confirming that the release tag is generated successfully.

Finally, push your changes to complete the task.

⚠️ Important Notes:

Perform all actions as the natasha user

Do not modify permissions or ownership of the repository or existing directories

🔹 Explanation of What You Need to Do
1️⃣ Understand the Repository Setup

/opt/official.git → Bare Git repository (hooks live here)

/usr/src/kodekloudrepos → Working clone where you edit and merge code

2️⃣ Merge the Feature Branch

You must merge the feature branch into master before pushing.

This ensures that all feature changes are included in the release.

3️⃣ Create a Post-Update Hook

A post-update hook runs automatically after a push happens on the repository.

Your hook should:

Detect when the master branch is updated

Generate today’s date

Create a Git tag named:

release-YYYY-MM-DD


This automates the release tagging process.

4️⃣ Test the Hook

After creating the hook:

Push changes to the master branch

Confirm that a new release tag is created for today

This proves that the hook works correctly.

5️⃣ Push the Changes

Once everything is verified:

Push the merged master branch

Ensure the tag exists in the repository

🔹 Key Concepts Tested in This Task
Concept	Why it matters
Bare repository	Hooks must be configured here
post-update hook	Runs after push
Git tags	Used for releases
Branch merge	Feature → master workflow
Automation	Real DevOps practice
✅ One-Line Summary

Merge feature into master, create a post-update Git hook that automatically tags every push to master with today’s release date, test it, and push the changes — all as user natasha without changing permissions.