# DISABLE OR ENABLE SSH ROOT LOGIN (Commands + Explanation)

# What is “Disable or Enable SSH Root Login”?

SSH Root Login = allowing the root user to log in directly via SSH.

/etc/ssh/sshd_config

### ==============================================================================

ls -lah /etc/ssh               # List SSH configuration directory contents

ssh kyaw@hostname              # SSH into server using hostname
ssh kyaw@ip                    # OR using IP address

vi /etc/ssh/sshd_config        # Open SSH daemon config file in editor (vi)

# INSIDE VI EDITOR:
 /PermitRootLogin              # Search for "PermitRootLogin" (type slash / then text)
 
# MODIFY THESE VALUES:
 PermitRootLogin yes           # Allow root login over SSH (ENABLE)
 PermitRootLogin no            # Deny root login over SSH (DISABLE)

# VI TEXT EDITING:
 $                             # Jump to end of line
 A                             # Append (enter insert mode at end of line)

# SAVE CHANGES IN VI:
 Esc :wq                       # Exit insert mode, Write & Quit

# APPLY THE CHANGES:
systemctl restart sshd         # Restart SSH daemon to apply changes
systemctl status sshd          # Show SSH daemon status (running, failed, etc.)
