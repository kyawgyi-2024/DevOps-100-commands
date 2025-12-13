# Day-2 : TEMPORARY USER ACCOUNT WITH EXPIRY DATE (Commands + Explanation)

# What is a Temporary User Account with Expiry Date?

A temporary user account is a Linux/Unix user account that is created for a limited period and automatically disabled or deleted after a set expiry date.

Unlike regular accounts, the user cannot access the system after the expiry date.

The account expires automatically without manual intervention.

### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

ssh tony@ip                     # SSH to server as user tony (replace ip with real IP address)

sudo useradd -h                 # Display help for useradd
sudo useradd -h | grep expir    # Filter for expiration options

sudo useradd -e 2023-12-07 ammar   # Create user 'ammar' with expiry date 07 Dec 2023

# CHECK & MANAGE EXPIRATION WITH CHAGE

chage                           # Run chage for help info (interactive mode without params)
chage -l ammar                  # List password + account expiry for user ammar

sudo !!                         # Repeat last command with sudo (e.g. chage -l ammar)
sudo chage -l ammar             # Same as above but explicitly written


### ========================================================================================

# Create user tempuser that expires on Dec 20, 2025
sudo useradd -e 2025-12-20 tempuser
sudo passwd tempuser

# Verify expiry date
chage -l tempuser
