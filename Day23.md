# What is a Git Fork?
A Git fork is a personal copy of someone else’s repository that lives under your own account (usually on GitHub / GitLab / Bitbucket).

➡️ Fork = Server-side copy of a repository
➡️ Different from git clone (which is local)

🔁 Fork vs Clone (Very Important)

| Feature                   | Fork                    | Clone                 |
| ------------------------- | ----------------------- | --------------------- |
| Location                  | On Git hosting platform | On your local machine |
| Ownership                 | Your account            | Local system          |
| Can push to original repo | ❌ No (by default)       | ❌ No                  |
| Use case                  | Contribute safely       | Work locally          |

# Why Do We Use Git Fork?
1️⃣ To Contribute to Open-Source Projects
You cannot push directly to someone else’s repository.
✔ Fork → ✔ Make changes → ✔ Pull Request

2️⃣ To Work Without Permission
No access to original repo
Fork gives you full control over your own copy

3️⃣ To Experiment Safely
Try new features
Break things without risk
Original repo stays untouched

4️⃣ DevOps / Enterprise Use Case
Team-based development
Review code before merging
Enforce CI/CD checks

🔄 Typical Fork Workflow (Step-by-Step)
1️⃣ Fork the repository (UI action)
On GitHub / GitLab:
Click Fork
Repo is copied to your account

2️⃣ Clone your fork locally
git clone https://github.com/your-username/project.git

3️⃣ Add original repo as upstream
git remote add upstream https://github.com/original-owner/project.git

4️⃣ Create a new branch
git checkout -b feature-branch

5️⃣ Push changes to your fork
git push origin feature-branch

6️⃣ Create Pull Request (PR)
From your fork → original repo
Maintainers review and merge

🔁 Keeping Your Fork Updated
git fetch upstream
git merge upstream/main

🔐 Fork in DevOps & CI/CD
Fork triggers CI pipelines
PR requires:
Code review
Tests
Security checks
Only approved code reaches production

🎯 Real-World Example
GitHub open-source project
You want to fix a bug
❌ No push access
✅ Fork → Fix → PR → Merge

📝 Interview One-Line Answer
A Git fork is a server-side copy of a repository that allows users to experiment and contribute without affecting the original project.

🚨 Common Mistake (Important)
❌ Fork ≠ Clone
✔ Fork is on GitHub
✔ Clone is on your machine