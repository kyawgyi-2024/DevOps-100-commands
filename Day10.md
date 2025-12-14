### Day-10: Linux Bash Scripts (Backup Example)

# Connect to Server and Prepare Script

ssh name@ip_or_hostname                        # SSH login to server
# Enter password and type 'yes' if prompted

cd /scripts                                    # Go to scripts directory
ls                                             # List files
touch blog_backup.sh                            # Create empty script file
ls -lah                                        # Verify script creation

chmod +x blog_backup.sh                         # Add execute permission
ls -lah                                        # Confirm permissions

# Edit Script File

vi blog_backup.sh                               # Open script in editor

# Example script content:

#!/bin/bash

# Backup file name
BK_FILE="xfusioncorp_blog.zip"

# Source directory to backup
SOURCE_DIR="/var/www/html/blog"

# Local backup directory
BK_DIR="/backup"

# Remote server details
SRV_DIR="/backup"
SSH_USER="clint"
SSH_HOST="stbkp01"

# Create backup zip file
zip -r "${BK_DIR}/${BK_FILE}" "${SOURCE_DIR}"

# Copy backup to remote server
scp "${BK_DIR}/${BK_FILE}" ${SSH_USER}@${SSH_HOST}:${SRV_DIR}

# Save and exit:
# Esc -> :wq -> Enter

# Run Script (Password Required Initially)
./blog_backup.sh                               # Run backup script
# Will prompt for remote server password

# Verify Backup on Remote Server
ssh clint@stbkp01                              # Login to backup server
cd /backup/                                     # Navigate to backup folder
ls                                             # List backup file

# Enable Passwordless SSH (Optional but Recommended)

Generate SSH key on source server:
ssh-keygen                                     # Press Enter for default file and no passphrase

# Login again from source server to test:
ssh clint@stbkp01                             # Should not require password now

# Run Script Automatically (No Password Needed)
cd /scripts
./blog_backup.sh                               # Runs without prompting for password

# Adjust Permissions on Backup Server
sudo chown -R steve /backup/                  # Change ownership to 'steve' user
ls -lah /backup/                               # Verify ownership and permissions


# Explanation of Commands

1. ssh name@ip → Connect to source server.

2. touch + chmod +x → Create and make script executable.

3. vi → Edit script with Bash commands for backup and SCP copy.

4. zip -r → Create a compressed backup of the source directory.

5. scp → Copy the backup file to remote server.

6. ssh-keygen → Generate SSH key for passwordless login.

7. chown -R → Change ownership of backup files for proper access.

### -------------------------------------------------------------------------

# What is a Backup Bash Script?

A Backup Bash Script is a Linux shell script written in Bash that automates the process of backing up files, directories, or databases.

It can compress files, copy them locally or to a remote server, and set permissions automatically.

Typically uses commands like zip, scp, rsync, and tar.

### -------------------------------------------------------------------------

# Why Use a Backup Bash Script?

| Reason              | Explanation                                                               |
| ------------------- | ------------------------------------------------------------------------- |
| **Automation**      | Eliminates the need to manually back up files every time.                 |
| **Consistency**     | Ensures the same backup steps are performed every time.                   |
| **Efficiency**      | Compresses and transfers files quickly, saving time and space.            |
| **Remote Backup**   | Can copy backups to another server automatically.                         |
| **Scheduling**      | Can be run via cron jobs for regular backups without manual intervention. |
| **Error Reduction** | Reduces human mistakes compared to manual backup.                         |

### -------------------------------------------------------------------------

A Backup Bash Script automates, secures, and simplifies backing up data on Linux servers.

# Backup Bash Script:
A Bash script used to automate file or directory backups on Linux, reducing manual work and preventing data loss.


### -------------------------------------------------------------------------

# What is zip?

zip is a command used to compress files or directories into a .zip file.

Why use zip?

1. Reduce file size

2. Easy to transfer backups

3. Common and widely supported format

zip backup.zip file.txt                    # Compress a single file
zip -r backup.zip /var/www/html            # Compress a directory (recursive)
unzip backup.zip                            # Extract zip file

Used mostly for simple backups and transfers.

### -------------------------------------------------------------------------

# What is scp?        # scp (Secure Copy)

scp securely copies files between local and remote servers using SSH.

Why use scp?

1. Encrypted file transfer

2. Simple and fast

3. Uses SSH authentication (keys or password)

scp file.txt user@server:/path/             # Copy local → remote
scp user@server:/path/file.txt ./           # Copy remote → local
scp -r /backup user@server:/backup          # Copy directory recursively

### -------------------------------------------------------------------------

# What is rsync?        # rsync

rsync is a smart file synchronization tool that copies only changed data.

# Why use rsync?

1. Faster than scp

2. Saves bandwidth

3. Ideal for frequent backups

4. Can resume interrupted transfers

rsync -av /data/ /backup/                   # Local sync
rsync -av /data/ user@server:/backup/       # Local → remote sync
rsync -av --delete /data/ /backup/          # Mirror source to destination

Best tool for production backups.

### -------------------------------------------------------------------------

# What is tar?

tar is used to archive files/directories into a single file (.tar, .tar.gz).

# Why use tar?

1. Preserves file permissions

2. Efficient for Linux backups

3. Common in system-level backups

tar -cvf backup.tar /var/www/html            # Create tar archive
tar -xvf backup.tar                          # Extract tar file

tar -czvf backup.tar.gz /data                # Create compressed tar
tar -xzvf backup.tar.gz                      # Extract compressed tar

Most used for system & server backups.

### -------------------------------------------------------------------------

| Tool    | Purpose        | Best Use Case         |
| ------- | -------------- | --------------------- |
| `zip`   | Compress files | Simple backups        |
| `scp`   | Secure copy    | One-time transfers    |
| `rsync` | Sync files     | Regular/large backups |
| `tar`   | Archive files  | Linux system backups  |

-------------------------------------------------------------------------


zip → Compress files into .zip

scp → Secure file copy over SSH

rsync → Efficient incremental backup tool

tar → Archive and compress Linux files

### -------------------------------------------------------------------------