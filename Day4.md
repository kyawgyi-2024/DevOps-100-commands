# SCRIPT EXECUTION PERMISSIONS (ALLOW EXECUTION OF SCRIPT)

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




