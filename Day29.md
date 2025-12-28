# DevOps Day-29: Manage Git Pull Requests (Gitea)

🔹 Objective
Prevent direct pushes to the master branch
Use feature branches and Pull Requests (PRs)
Ensure code is reviewed and approved before merging
Simulate a real-world DevOps workflow

🔹 Scenario Overview
Max writes a new story: 🦊 Fox and Grapes 🍇
Max pushes his changes to a feature branch
story/fox-and-grapes
The master branch must remain protected
Changes must be merged only via Pull Request
Tom will review and approve the PR
----------------------------------------------------------------------------------------------------------

ssh max@storage-server
# Login to the storage server as user 'max'
# Password: Max_pass123

ls
# List files and directories in Max's home directory

cd story-blog/
# Move into the already cloned Git repository

git status
# Check current branch and working tree status
# Confirms no uncommitted changes

git branch
# List all local branches
# Confirms current branch is 'story/fox-and-grapes'

git remote -v
# Verify remote repository (Gitea) URL for fetch and push

git log
# View commit history
# Used to confirm:
# - Sarah's story exists
# - Max's commit is present
# - Author, commit message, and timestamps are correct

----------------------------------------------------------------------------------------------------------

# Pull Request (PR) Creation – Gitea UI
Performed via web UI, not CLI
Login to Gitea UI
Username: max
Password: Max_pass123

# Create Pull Request
PR Title: Added fox-and-grapes story
Source Branch: story/fox-and-grapes
Target Branch: master

# Purpose:
Prevent direct push to master
Ensure code review before merge
Assign Reviewer
Reviewer: tom
Adds peer review requirement before merge
Review & Merge – As Tom
Login to Gitea UI
Username: tom
Password: Tom_pass123

Open PR: Added fox-and-grapes story
Review changes
Approve PR
Merge into master
Ensures reviewed and approved code reaches master
Notes (Important for Interview)

master branch → Production-ready code only
Feature branches → Safe development environment
Pull Requests → Code review + collaboration
Reviewer approval → Quality & security control
No direct push to master → DevOps best practice