## DevOps Day-22: Clone Git Repository on Storage Server (One-Sheet)

### 🔹 Login to Storage Server
ssh natasha@ststor01                 # Login to storage server as user 'natasha'

### 🔹 Verify Source Bare Repository
ls /opt/cluster.git                 # Check that the bare Git repository exists

**Explanation:**
* `/opt/cluster.git` is a **bare repository** created earlier
* Bare repos are used as central sources for cloning

### 🔹 Prepare Destination Directory
ls -lah /usr/src/kodekloudrepos      # Check destination directory permissions and contents
cd /usr/src/kodekloudrepos           # Move into directory where repo will be cloned

### 🔹 Clone the Repository
sudo git clone /opt/cluster.git
# Clone the bare repository into a working directory
# sudo is used because /usr/src may require root permissions
ls                                  # Verify that 'cluster' directory is created

### 🔹 Inspect the Cloned Repository
cd cluster/                          # Enter cloned repository
ls -lah                              # List project files and .git directory
git status                           # Check current branch and working tree status
git remote -v                        # Display remote repository URLs

**Explanation:**
* Confirms the repo is correctly linked to `/opt/cluster.git`
* Shows fetch and push URLs

### 🔹 Fix Git Safe Directory Warning
git config --global --add safe.directory /usr/src/kodekloudrepos/cluster
# Mark repository as safe when owned by different user (common with sudo clones)
git remote -v                        # Recheck remotes after fixing safe directory issue

### 📝 Summary
* Bare repository verified in `/opt`
* Repository cloned into `/usr/src/kodekloudrepos`
* Working directory created from bare repo
* Git status and remotes verified
* Safe directory configured to avoid permission warnings

✔ Ready for development, builds, or deployment pipelines
