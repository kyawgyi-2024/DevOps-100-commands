## DevOps Day-21: Set Up Git Repository on Storage Server (One-Sheet)
A bare repository is a special kind of Git repository without a working directory (no project files you can edit).
Bare repo = storage only
Normal repo = work + storage

### 🔹 Login to Storage Server
ssh natasha@ststor01                 # Login to storage server as user 'natasha'

sudo yum install -y git              # Install Git version control system
ls                                  # Verify current directory contents

### 🔹 Create a Bare Git Repository (Central Repository)

git init --bare                     # Initialize a bare Git repository in current directory
ls                                  # Check repository files (HEAD, objects, refs, etc.)

### 🔹 Best Practice: Store Repositories in /opt

cd /opt                             # Move to /opt directory (commonly used for shared services)
ls -lah                             # List directory contents with details

sudo git init --bare blog.git       # Create a bare repository named blog.git

> `sudo !!` can be used to rerun the previous command with sudo

ls -lah /opt/blog.git               # Verify bare repository structure

=================================================================================================================

### 🔹 Git Knowledge: Normal Repo vs Bare Repo

#### ✅ Normal Git Repository
git init repo-name                  # Initialize a normal Git repository
ls -lah                             # Shows working directory files
ls -lah repo-name/                  # Repository folder
ls -lah repo-name/.git/             # Hidden .git directory (stores Git metadata)

**Characteristics:**
* Has a working directory
* Contains a `.git` folder
* Files can be edited and committed directly
* Used by developers locally

#### ✅ Bare Git Repository
git init --bare bare-repo           # Initialize a bare Git repository
ls -lah                             # Shows Git metadata files directly
ls -lah bare-repo/                  # No .git directory inside

**Characteristics:**
* ❌ No working directory
* ❌ No `.git` folder
* ❌ Cannot edit files directly
* ✔ Used only for push/pull operations
* ✔ Ideal for centralized repositories (Git server)

### 🔐 Why Use a Bare Repository?
* Acts as a **central storage** for code
* Prevents accidental file edits
* Safe for multi-user access
* Used by CI/CD pipelines and deployment servers

### 📝 Summary
* Git installed on storage server
* Bare repository created using `git init --bare`
* Bare repo contains only Git metadata
* Normal repo has working directory + `.git` folder
* Bare repositories are used only for centralized version control

✔ Perfect for DevOps Git servers and team collaboration

=============================================================================================================

A bare repository is a special kind of Git repository without a working directory (no project files you can edit).

What is a Bare Repository?
Normal Git repo (non-bare)

Has:
.git/ folder (Git metadata)
Working files (index.html, src/, etc.)
You edit code here

Example:
my-project/
 ├─ .git/
 ├─ index.html
 └─ app.js

Bare Git repo:
Has ONLY Git metadata
NO working files
Usually ends with .git

Example:
blog.git/
 ├─ HEAD
 ├─ objects/
 ├─ refs/
 └─ config
👉 You cannot edit files inside a bare repo.

Why Use a Bare Repository?
1️⃣ Used as a central/shared repository
Bare repos are perfect for:
Git servers
Storage servers
Team collaboration

Example:
Developer A  ---> push --->  bare repo (server)
Developer B  ---> pull --->  bare repo (server)

2️⃣ Safe for git push
If you push to a normal repo, Git may refuse because it would overwrite working files.
Bare repo:
No working files
Safe to push
No conflicts with checked-out branches

3️⃣ Standard practice on servers

On servers, you don’t code directly.
You:
Push from your laptop
Pull from server when needed

That’s why GitHub, GitLab, Bitbucket all use bare repos internally.

When SHOULD you use a Bare Repo?
✅ On:
Storage server
Git server
DevOps setup
Team projects

❌ NOT for:
Writing code
Local development

Simple Example (Your DevOps Context)
You did:
sudo git init --bare blog.git

That means:
blog.git is a central repo

Developers will do:
git clone natasha@ststor01:/opt/blog.git


| Feature        | Normal Repo | Bare Repo |
| -------------- | ----------- | --------- |
| Working files  | ✅ Yes       | ❌ No      |
| Can edit code  | ✅ Yes       | ❌ No      |
| Used on server | ❌ Rare      | ✅ Yes     |
| Safe for push  | ⚠️ Risky    | ✅ Safe    |

