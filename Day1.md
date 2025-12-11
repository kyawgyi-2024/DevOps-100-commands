# NON-INTERACTIVE SHELLS (Commands + Explanation)

# What is a Non-Interactive Shell?
A non-interactive shell is a shell session that runs commands automatically without requiring user input.
Unlike an interactive shell, it does not display a prompt ($ or #) or wait for you to type commands.
Commonly used for scripts, cron jobs, remote commands, and automation.
# Non-interactive shells are essential for automation and scheduled tasks because they allow commands to run without human interaction.

### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

/sbin/nologin       # Denies login, shows “This account is currently not available.”
/usr/sbin/nologin   # Same as above (different path depending on OS)
/bin/false          # Denies login instantly, no message

# CHECK IF THESE SHELLS EXIST
ls /sbin/nologin         # Check for nologin in /sbin
ls /usr/sbin/nologin     # Check for nologin in /usr/sbin
ls -lah /bin/false       # Show detailed information for /bin/false

# USERADD HELP
useradd -h               # Show useradd help options
useradd -h | grep shell  # Filter output to lines containing “shell”

# SSH LOGIN
ssh steve@ip             # SSH to server as user steve (replace ip with real address)

# CREATE USER WITH NO LOGIN ACCESS (SERVICE USERS)
sudo useradd -s /sbin/nologin kirsty        # Create user kirsty with no shell
sudo useradd -s /usr/sbin/nologin kirsty    # Same as above depending on distro
sudo useradd -s /bin/false kirsty           # Strong deny, no message to user

# VERIFY USER SHELL
grep kirsty /etc/passwd   # Check shell assigned to kirsty
