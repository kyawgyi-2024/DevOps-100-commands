### Day-8: Install Ansible on Linux

# What is Ansible?

Ansible is an open-source automation tool used to configure, manage, and deploy applications or infrastructure on multiple servers from a central location.

a) It uses SSH (or APIs) to connect to servers.

b) It is agentless — no need to install software on target servers.

c) Configurations are written in YAML files called playbooks.

### =====================================================================================================

# Install Ansible on Linux

which pip3                                    # Check Python 3 pip location
# Output example: /usr/local/bin/pip3

sudo pip3 install ansible==4.7.0              # Install Ansible (specific version 4.7.0)
# Enter jump host password if prompted (thor), type 'yes' to confirm host fingerprint

ansible --version                             # Verify installed Ansible version
# Example output: ansible [core 2.11.12]

which ansible                                 # Check exact location of ansible binary
# Example output: /usr/local/bin/ansible

echo $PATH                                    # Check current PATH environment variable

sudo ansible --version                         # Check Ansible for root user
# May show: sudo: ansible: command not found

sudo visudo                                   # Edit sudoers file to update secure PATH
# Look for line starting with: Defaults secure_path          # /_path
 Default: secure_path = /sbin:/bin:/usr/sbin:/usr/bin        # default path
 Update it to include /usr/local/bin:
 secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin  # update path
# Save and exit: Esc -> :wq -> Enter

sudo ansible --version                         # Recheck Ansible version as root
# Should now show the installed version

# Jump host login (example)
ssh thor@jumphost                              # Connect to jump host
# Enter password: mjolnir123

===========================================================================================

# Key Notes & Explanations

1. which pip3 → Ensures Python3 and pip3 are installed.

2. sudo pip3 install ansible==4.7.0 → Installs a specific Ansible version system-wide.

3. ansible --version → Confirms the installed Ansible version.

4. which ansible → Tells exactly where the Ansible executable is located.

5. echo $PATH → Shows directories searched for commands; needed to run Ansible as sudo.

6. sudo ansible --version → Fails if /usr/local/bin is not in root's secure PATH.

7. sudo visudo → Safely edit sudoers file to add /usr/local/bin to secure_path.

8. Always use Esc -> :wq -> Enter to save sudoers changes.

9. After updating PATH, root can run sudo ansible successfully.

### =====================================================================================================

# Why Use Ansible?

| Reason            | Explanation                                                                        |
| ----------------- | ---------------------------------------------------------------------------------- |
| **Automation**    | Automates repetitive tasks like software installation, updates, and configuration. |
| **Consistency**   | Ensures all servers are configured the same way.                                   |
| **Agentless**     | No need to install extra software on target servers; uses SSH.                     |
| **Easy to Learn** | YAML syntax is simple and human-readable.                                          |
| **Scalable**      | Can manage hundreds or thousands of servers easily.                                |
| **Integration**   | Works well with CI/CD pipelines and cloud platforms.                               |

### =====================================================================================================

# Ansible helps save time, reduce errors, and manage infrastructure efficiently by automating tasks across multiple servers.

# Ansible:
An open-source automation tool for configuring, deploying, and managing servers using agentless SSH.

# Why use it:

Automates tasks, ensures consistency, and saves time.

Easy to learn (YAML), scalable, and integrates with CI/CD pipelines.