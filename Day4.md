# SCRIPT EXECUTION PERMISSIONS (ALLOW EXECUTION OF SCRIPT) (Commands + Explanation)

# What is SCRIPT EXECUTION PERMISSIONS?

In Linux, script execution permission means giving a file the right to be executed (run) like a program.

This permission is controlled by the execute bit (x).

### =======================================================================================

ssh koko@host                  # SSH into server using hostname
ssh koko@ip                    # OR using IP address

ls -lah /tmp                   # List /tmp directory with detailed permissions

sudo -i                        # Enter root shell (admin mode)

sudo chmod a+rx /tmp/devops.sh   # Give ALL users Read + Execute permissions on script
                                 # a = all users
                                 # +r = add read
                                 # +x = add execute

ls -lah /tmp                   # Verify devops.sh has correct permissions (look for 'x')

# Example result:
# -r-xr-xr-x  1 root root   1.2K   date  devops.sh
#
# meaning: executable by everyone

### ===========================================================================

# FILE OWNERSHIP (chown / chgrp)

chown user file                  # Change file owner to user
chown koko /tmp/devops.sh        # Make koko the owner of devops.sh

chgrp group file                 # Change file group ownership
chgrp sysadmin /tmp/devops.sh    # Change group to sysadmin

chown user:group file            # Change BOTH owner and group
chown koko:sysadmin /tmp/devops.sh

# RECURSIVE OWNERSHIP (for directories)
chown -R user:group /var/www     # Apply to all files/directories inside

# VIEW OWNERSHIP
ls -lah /tmp                     # Shows owner & group of each file

# EXAMPLE OUTPUT:
# -r-xr-xr-x 1 koko sysadmin 1.2K date devops.sh
#
# koko     = file owner
# sysadmin = file group


### ===================================================================================

# CHMOD NUMERIC PERMISSIONS

# PERMISSION VALUES:
r = 4     # read
w = 2     # write
x = 1     # execute

# OWNER / GROUP / OTHERS
chmod 755 file                 # Owner: rwx (7), Group: r-x (5), Others: r-x (5)
                              # Common for executable scripts & web files

chmod 644 file                 # Owner: rw- (6), Group: r-- (4), Others: r-- (4)
                              # Common for text files & configs (NOT executable)

chmod 700 file                 # Owner: rwx (7), Group: --- (0), Others: --- (0)
                              # Owner only, used for private scripts

chmod 600 file                 # Owner: rw- (6), Group: --- (0), Others: --- (0)
                              # Used for SSH private keys (id_rsa)

chmod 777 file                 # Owner: rwx, Group: rwx, Others: rwx
                              # Everyone full access — NOT recommended (security risk)

chmod 440 file                 # Owner: r--, Group: r--, Others: ---
                              # Read-only by owner & group

# RECURSIVE PERMISSIONS
chmod -R 755 /var/www         # Apply to directory and all inside


### =========================================================================================
# LINUX FILE PERMISSIONS (r, w, x)

r = read                       # Can view file contents / list directory
w = write                      # Can modify file / add & delete inside directory
x = execute                    # Can run the file / access directory

# PERMISSIONS BREAKDOWN
                FILE                         DIRECTORY
r (4)   read file contents           list directory contents (ls)
w (2)   modify file contents         create/delete/rename files inside
x (1)   execute/run file             access/enter directory (cd)

# EXAMPLES
-rw-r--r--    file.txt
              │  │  └ others: read
              │  └ group: read
              └ owner: read & write

-rwxr-xr-x    script.sh
              │ │  └ others: read + execute
              │ └ group: read + execute
              └ owner: read + write + execute


### =========================================================================================

# SPECIAL PERMISSION BITS (SUID / SGID / STICKY BIT)

# SUID (Set User ID) — run as file OWNER
chmod u+s file
# Example:
chmod u+s /usr/bin/passwd     # Users can change passwords using root privilege
# Permission shows as:
-rwsr-xr-x                    # 's' replaces 'x' for owner

# SGID (Set Group ID) — run as file GROUP / or inherit group on directories
chmod g+s directory_or_file
# For directory:
chmod g+s /shared
# New files inside /shared inherit the group of the directory
# Permission shows as:
rwxr-sr-x                     # 's' replaces 'x' for group

# STICKY BIT — prevent file deletion by others in same directory
chmod +t directory
# Common use:
chmod +t /tmp
# Permission shows as:
rwxrwxrwt                     # 't' at the end
# Meaning: users can only delete their own files inside /tmp

# SUMMARY
SUID  = execute file as owner (common for system utilities)
SGID  = execute as group / preserve group ownership
t     = sticky bit — users can’t delete each other’s files

# VIEW SPECIAL BITS
ls -lah
# Look for:  s (SUID/SGID)  or  t (sticky bit)


### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# DIRECTORY vs FILE PERMISSIONS (IMPORTANT DIFFERENCE)

# ON A FILE:
r = can read contents
w = can edit/modify contents
x = can execute the file (if script/binary)

# ON A DIRECTORY:
r = can list directory contents (ls)
w = can create / delete / rename files inside
x = can enter directory (cd) and access file names

# KEY EXAMPLES

# Example 1 — directory without x
drw-r--r--  dir
# Can NOT cd into it — no execute permission
# ls allowed? Yes
# cd allowed?  NO

# Example 2 — directory without r
d--x--x--x  dir
# Can cd into it
# Cannot list files (ls fails: Permission denied)
# BUT if you know filename, you can access it

# Example 3 — directory with w but no x
dw-w--w--w  dir
# CANNOT create files (need x also)
# Write on directory always requires BOTH: w + x

# Example — execute on a DIR means:
cd dir        # can enter
ls dir        # IF read is present
open files    # IF you know their names

# SUMMARY COMPARISON:
FILE r   = read file
DIR  r   = list directory

FILE w   = edit file
DIR  w   = add / remove files

FILE x   = run file
DIR  x   = enter directory + access names


### ------------------------------------------------------------------------------------------

# OWNERSHIP & PERMISSION TROUBLESHOOTING CHECKLIST

# 1) CHECK WHO OWNS THE FILE
ls -lah file
# Look at: owner & group

# 2) CHECK CURRENT USER & GROUPS
whoami             # shows current user
id                 # shows user + groups that user belongs to

# 3) VERIFY PERMISSIONS
ls -lah file       # look for rwx or --- values

# 4) CHECK IF DIRECTORY HAS ACCESS
# Remember:
# Directory requires x permission to enter
# Directory requires r permission to list
# Directory requires w+x to create/delete files

# 5) CHANGE OWNERSHIP IF NEEDED
sudo chown user file
sudo chown user:group file

# 6) FIX PERMISSIONS
chmod 644 file          # good for normal files
chmod 755 script.sh     # allow execution
chmod 700 private.key   # secure private key

# 7) CHECK SPECIAL BITS
# Look for s or t flags
ls -lah directory

# 8) IF PERMISSION DENIED:
# Check parent directories:
ls -ld .
ls -ld ..
ls -ld /path

# 9) REMEMBER: user must have
# permission on ALL parent directories to reach a file

# 10) CHECK SELINUX (if enabled)
sestatus
# If enforcing, SELinux rules may block access even if permissions look correct

# 11) CHECK FILESYSTEM ATTRIBUTES
lsattr file
# Flags like 'i' may prevent modification

# 12) VERIFY SCRIPT EXECUTABLE
chmod +x script.sh
./script.sh

# 13) CHECK PATH PROBLEMS
which script.sh
echo $PATH

# 14) WHEN IN DOUBT — test as root
sudo -i
# If root can access but normal user cannot → permission problem


### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# LINUX PATHS: ABSOLUTE vs RELATIVE

# 1) ABSOLUTE PATH
# - Full path from root "/"
# - Always starts with /
# - Independent of current working directory

EXAMPLES:
ls /etc/ssh/sshd_config       # absolute path
cat /var/log/syslog           # absolute path
/home/koko/devops.sh           # absolute path

# 2) RELATIVE PATH
# - Path relative to current working directory
# - Does NOT start with /
# - Depends on where you currently are

EXAMPLES:
ls sshd_config                # if inside /etc/ssh
cat ../syslog                 # go up one directory and read syslog
./devops.sh                   # current directory script execution

# 3) SPECIAL RELATIVE SYMBOLS
.       → current directory
..      → parent directory
~       → home directory of current user
-       → previous working directory

# 4) CHECK CURRENT WORKING DIRECTORY
pwd                            # prints full path of current directory

# 5) NAVIGATION
cd /var/log                     # go to absolute path
cd ..                            # go up one level
cd ~                             # go to home directory
cd -                             # go to previous directory

# 6) QUICK TIPS
# - Absolute paths are safer in scripts
# - Relative paths are convenient for moving within project directories
# - Use ./ to explicitly run a file in current directory


### ------------------------------------------------------------------------------------------

# UMASK & DEFAULT PERMISSION CREATION

# 1) WHAT IS UMASK?
# - Umask = User file creation MASK
# - Determines which permissions are **disabled** by default
# - Subtracted from default permissions to create new files/directories

# 2) DEFAULT PERMISSIONS
File default = 666 (rw-rw-rw-)      # read & write for all (no execute)
Directory default = 777 (rwxrwxrwx) # read, write, execute for all

# 3) CALCULATE FINAL PERMISSIONS
Final permission = Default - Umask

EXAMPLES:
umask 022
# Files: 666 - 022 = 644 (rw-r--r--)
# Directories: 777 - 022 = 755 (rwxr-xr-x)

umask 077
# Files: 666 - 077 = 600 (rw-------)
# Directories: 777 - 077 = 700 (rwx------)
# Used for private files

# 4) CHECK CURRENT UMASK
umask
# Returns current mask in octal (e.g., 0022)

# 5) TEMPORARILY CHANGE UMASK
umask 027   # affects files/directories created in current shell session

# 6) PERSISTENT UMASK CHANGE
# Add line to shell config (~/.bashrc, /etc/profile, /etc/login.defs)
umask 022

# 7) QUICK REFERENCE TABLE
Umask | File Perm | Dir Perm
----------------------------
000   | 666        | 777
022   | 644        | 755
027   | 640        | 750
077   | 600        | 700
002   | 664        | 775   # for shared group collaboration


### ******************************************************************************************

# UMASK & DEFAULT PERMISSION CREATION

# 1) WHAT IS UMASK?
# - Umask = User file creation MASK
# - Determines which permissions are **disabled** by default
# - Subtracted from default permissions to create new files/directories

# 2) DEFAULT PERMISSIONS
File default = 666 (rw-rw-rw-)      # read & write for all (no execute)
Directory default = 777 (rwxrwxrwx) # read, write, execute for all

# 3) CALCULATE FINAL PERMISSIONS
Final permission = Default - Umask

EXAMPLES:
umask 022
# Files: 666 - 022 = 644 (rw-r--r--)
# Directories: 777 - 022 = 755 (rwxr-xr-x)

umask 077
# Files: 666 - 077 = 600 (rw-------)
# Directories: 777 - 077 = 700 (rwx------)
# Used for private files

# 4) CHECK CURRENT UMASK
umask
# Returns current mask in octal (e.g., 0022)

# 5) TEMPORARILY CHANGE UMASK
umask 027   # affects files/directories created in current shell session

# 6) PERSISTENT UMASK CHANGE
# Add line to shell config (~/.bashrc, /etc/profile, /etc/login.defs)
umask 022

# 7) QUICK REFERENCE TABLE
Umask | File Perm | Dir Perm
----------------------------
000   | 666        | 777
022   | 644        | 755
027   | 640        | 750
077   | 600        | 700
002   | 664        | 775   # for shared group collaboration


### ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

# LINUX LINKS: HARD LINK vs SOFT LINK

# 1) HARD LINK
# - Another name for the same file on disk
# - Shares same inode number as original file
# - Cannot cross file systems (must be on same partition)
# - Cannot link directories (usually)
# - Deleting original file does NOT affect hard link

# COMMAND
ln original_file hardlink_name
# Example:
ln /tmp/devops.sh /tmp/devops_hard.sh

# VERIFY
ls -li /tmp/devops.sh /tmp/devops_hard.sh
# Same inode number = same file

# 2) SOFT LINK (SYMBOLIC LINK)
# - Pointer to original file (like a shortcut)
# - Can cross file systems
# - Can link directories
# - Deleting original file breaks the link (shows “dangling”)

# COMMAND
ln -s original_file softlink_name
# Example:
ln -s /tmp/devops.sh /tmp/devops_soft.sh

# VERIFY
ls -l /tmp/devops_soft.sh
# Shows: devops_soft.sh -> devops.sh

# 3) QUICK COMPARISON

Feature            | HARD LINK             | SOFT LINK
------------------ | -------------------- | -----------------
Inode              | Same as original      | Different inode
Cross FS            | No                   | Yes
Link dirs           | No                   | Yes
Delete original     | Hard link still works | Link breaks
Command             | ln file link          | ln -s file link

# 4) EXAMPLES
# Hard link
ln /home/koko/file.txt /home/koko/file_hard.txt

# Soft link
ln -s /home/koko/file.txt /home/koko/file_soft.txt


### $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

# USER MANAGEMENT

# Create normal user
sudo useradd username
sudo passwd username

# Create user with non-login shell
sudo useradd -s /sbin/nologin kirsty

# Create temporary user with expiry
sudo useradd -e 2023-12-07 ammar
chage -l ammar          # Check expiry info

# Verify user shell
grep username /etc/passwd

# SSH login
ssh user@hostname_or_ip


### ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# SSH ROOT LOGIN CONFIG

ls -lah /etc/ssh
vi /etc/ssh/sshd_config
# Search: /PermitRootLogin
PermitRootLogin yes       # enable root login
PermitRootLogin no        # disable root login

# VI shortcuts
$  → end of line
A  → insert at end
Esc :wq → save & exit

# Apply changes
sudo systemctl restart sshd
systemctl status sshd

### ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# SCRIPT EXECUTION & PERMISSIONS

ls -lah /tmp
sudo -i
sudo chmod a+rx /tmp/devops.sh
ls -lah /tmp   # verify permissions

# FILE OWNERSHIP

# Change owner
sudo chown user file
# Change group
sudo chgrp group file
# Change both
sudo chown user:group file
# Recursive
sudo chown -R user:group /dir
# Verify ownership
ls -lah file

# FILE PERMISSIONS (r, w, x)

# Read = r, Write = w, Execute = x
# File:
r = read content
w = modify content
x = run file
# Directory:
r = list contents
w = add/remove files
x = enter directory

# Example
-rwxr-xr-x  script.sh


# CHMOD NUMERIC MODES

# Octal notation: Owner | Group | Others
# r=4, w=2, x=1

chmod 755 file      # rwx r-x r-x
chmod 644 file      # rw- r-- r--
chmod 700 file      # rwx --- ---
chmod 600 file      # rw- --- ---
chmod 777 file      # rwx rwx rwx  (unsafe)


# SPECIAL BITS

# SUID
chmod u+s file
# Example: /usr/bin/passwd → -rwsr-xr-x

# SGID
chmod g+s dir_or_file
# Directory: new files inherit group → rwxr-sr-x

# Sticky Bit
chmod +t /tmp
# Only file owner can delete files → rwxrwxrwt


# UMASK & DEFAULT PERMISSIONS

# Default file = 666, dir = 777
# Final perms = default - umask

umask 022
# Files: 666-022 = 644
# Dirs: 777-022 = 755

umask 077
# Files: 600, Dirs: 700

# Check umask
umask
# Temporarily change
umask 027


# LINKS

# Hard Link (same inode)
ln original_file hardlink_name
ls -li original_file hardlink_name

# Soft Link / Symbolic Link
ln -s original_file softlink_name
ls -l softlink_name
# Shows: softlink_name -> original_file

# Hard vs Soft Summary
Feature | HARD | SOFT
Inode   | same | different
Cross FS| no   | yes
Dirs    | no   | yes
Delete orig | link works | link broken


# QUICK NAVIGATION & PATHS

# Absolute path = /home/user/file
# Relative path = ./file or ../file
pwd        # current directory
cd /path   # absolute
cd ..      # parent
cd ~       # home
cd -       # previous directory





