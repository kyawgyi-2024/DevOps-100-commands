# TEMPORARY USER ACCOUNT WITH EXPIRY DATE

ssh tony@ip                     # SSH to server as user tony (replace ip with real IP address)

sudo useradd -h                 # Display help for useradd
sudo useradd -h | grep expir    # Filter for expiration options

sudo useradd -e 2023-12-07 ammar   # Create user 'ammar' with expiry date 07 Dec 2023

# CHECK & MANAGE EXPIRATION WITH CHAGE

chage                           # Run chage for help info (interactive mode without params)
chage -l ammar                  # List password + account expiry for user ammar

sudo !!                         # Repeat last command with sudo (e.g. chage -l ammar)
sudo chage -l ammar             # Same as above but explicitly written
