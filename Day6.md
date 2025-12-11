# Cron Job Setup (Commands + Explanation)

# Cron job = auto-run command based on time.

# Why Do We Use Cron Jobs?

# Automate Repeated Tasks

 # -To run tasks without typing manually:

 # -backups

 # -cleanup files

 # -database work

 # -rotate logs

 # -restart services

# Cron Format Reference
* * * * * command
│ │ │ │ │
│ │ │ │ └─ day of week (0-7, 0 or 7 = Sunday)
│ │ │ └── month (1-12)
│ │ └── day of month (1-31)
│ └── hour (0-23)
└── minute (0-59)

# cronie → package for cron jobs

# crond → daemon that executes scheduled tasks

# crontab -e → edit cron jobs for a user

# crontab -l → list current cron jobs

# /var/spool/cron/ → stores cron files per user

# 1. SSH to server
ssh ko@<IP-or-hostname>        # connect to remote server

# 2. Install cronie (cron service package)
sudo yum install -y cronie     # installs cron & crond service

# 3. Start and enable crond
sudo systemctl start crond     # start cron service
sudo systemctl enable --now crond   # enable at boot + start now 
sudo systemctl status crond    # confirm running

# 4. Root crontab check & edit
sudo crontab -l                # list current cron jobs (root)
sudo crontab -e                # edit root cron file

# (Inside editor: add the cron job)
*/5 * * * * echo hello > /tmp/cron_text    # runs every 5 min

# Save: Esc :wq
sudo crontab -l                # recheck job added

# 5. Verify cron file stored
sudo ls -lah /var/spool/cron/  # shows root cron file

# 6. Add cron job for Tony user
su - Tony                      # switch to Tony
crontab -e                     # edit Tony’s cron
* * * * * test                 # runs every minute (example)
# Save: Esc :wq
crontab -l                     # verify Tony’s cron

sudo ls -lah /var/spool/cron/  # shows Tony file exists

# 7. Remove Tony’s cron file
sudo rm /var/spool/cron/Tony  # delete Tony's crontab
sudo ls -lah /var/spool/cron/ # confirm removed

# 8. Verify output of root cron job
cat /tmp/cron_text             # should show: hello



### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Practical Cron Jobs (Commands + Explanation)

# 1️⃣ SSH to Server
ssh ko@<IP-or-hostname>          # connect to server

# 2️⃣ Install cron service
sudo yum install -y cronie       # installs cron daemon (crond)

# 3️⃣ Start & Enable cron daemon
sudo systemctl start crond       # start cron service
sudo systemctl enable --now crond # enable at boot + start now
sudo systemctl status crond      # verify running

# 4️⃣ Root Cron Jobs

# Every 5 minutes - write "hello" to file
sudo crontab -e
# Add:
*/5 * * * * echo hello > /tmp/cron_text
# Save: Esc → :wq
sudo crontab -l                  # verify

# Daily at midnight - backup database
0 0 * * * mysqldump -u root -p123 dbname > /backup/db_$(date +\%F).sql

# Every hour - clear /tmp files
0 * * * * rm -f /tmp/*

# Weekly Sunday 2 AM - reboot server
0 2 * * 0 /sbin/reboot

# Every 10 minutes - log disk usage
*/10 * * * * df -h > /var/log/disk_usage.log

# Daily 3 AM - delete logs older than 7 days
0 3 * * * find /var/log -type f -mtime +7 -delete

# Every 30 minutes - sync data to remote server
*/30 * * * * rsync -avz /data/ user@10.10.10.10:/remote_backup/

# Every minute - append system date to file (test cron)
* * * * * date >> /tmp/date_log

# First day of month 5 AM - run custom report
0 5 1 * * /usr/local/bin/report.sh

# Every 6 hours - restart Docker containers
0 */6 * * * docker restart $(docker ps -q)

# 5️⃣ User Cron Job (example: Tony)
su - Tony                        # switch to Tony
crontab -e                        # edit Tony's cron
# Example job: every minute run "test" command
* * * * * test
# Save: Esc → :wq
crontab -l                        # verify Tony's cron

# 6️⃣ Verify cron files
sudo ls -lah /var/spool/cron/     # shows root and Tony cron files

# 7️⃣ Remove Tony's cron
sudo rm /var/spool/cron/Tony
sudo ls -lah /var/spool/cron/     # confirm removed

# 8️⃣ Verify root cron output
cat /tmp/cron_text                # shows "hello" every 5 min

# Cron Format Reference

* * * * * command
│ │ │ │ │
│ │ │ │ └─ Day of week (0-7, 0 or 7 = Sunday)
│ │ │ └── Month (1-12)
│ │ └── Day of month (1-31)
│ └── Hour (0-23)
└── Minute (0-59)

