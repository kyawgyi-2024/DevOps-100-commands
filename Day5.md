### SELinux Installation & Configuration

# SSH into server (using IP or hostname)
ssh k@IP_ADDRESS
ssh k@hostname

# Become root user
sudo -i

# Check hostname
hostnamectl

# Check OS version
cat /etc/os-release

# Install SELinux packages (CentOS / RHEL)
sudo yum install selinux-policy-targeted selinux-policy-devel policycoreutils -y

# Check SELinux directory
ls /etc/selinux

# Open SELinux config file
sudo vi /etc/selinux/config

# Inside vi: search for SELINUX line
/SELINUX

# VI editing keys
$       # go to end of line
A       # insert at end
ESC     # escape (exit insert mode)
:wq     # save & quit

# Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

# Change SELinux mode (inside file)
SELINUX=enforcing   # default
SELINUX=disabled    # change to disable SELinux

# Verify SELinux mode in config file
grep ^SELINUX= /etc/selinux/config

(SELINUX = disabled)


### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Systemctl Service Management

# Check service status
systemctl status service_name

# Start a service
systemctl start service_name

# Stop a service
systemctl stop service_name

# Restart a service
systemctl restart service_name

# Reload service (without stopping)
systemctl reload service_name

# Enable service to start at boot
systemctl enable service_name

# Disable service from starting at boot
systemctl disable service_name

# Check if service is enabled
systemctl is-enabled service_name

# Check if service is active (running)
systemctl is-active service_name

# View service logs (journal)
journalctl -u service_name

# Follow logs in real-time
journalctl -u service_name -f

# List all services
systemctl list-units --type=service

# List failed services
systemctl --failed

# Reload systemd daemon (after editing unit files)
systemctl daemon-reload

# Mask a service (disable completely)
systemctl mask service_name

# Unmask a service
systemctl unmask service_name


### ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Journalctl Log Management

# View all logs (full system log)
journalctl

# View logs for a specific service
journalctl -u service_name

# View logs and follow in real-time (like tail -f)
journalctl -u service_name -f

# View logs for current boot
journalctl -b

# View logs for previous boot
journalctl -b -1

# Show the last 100 lines of logs
journalctl -n 100

# Follow logs globally in real-time
journalctl -f

# Filter logs by time
journalctl --since "1 hour ago"
journalctl --since today
journalctl --since "2024-01-01" --until "2024-01-02"

# Filter logs by priority
journalctl -p err        # only errors
journalctl -p warning    # only warnings
journalctl -p crit       # critical
journalctl -p emerg      # emergency

# Filter logs by specific process ID (PID)
journalctl _PID=1234

# Filter logs by user
journalctl _UID=1000

# Filter logs by executable binary
journalctl /usr/bin/nginx

# View kernel logs only
journalctl -k

# Clear logs (dangerous; requires root)
journalctl --rotate
journalctl --vacuum-time=7d      # keep last 7 days
journalctl --vacuum-size=500M    # keep only 500MB logs

# Export logs
journalctl > all_logs.txt
journalctl -u service_name > service_logs.txt

# Show logs with nice formatting
journalctl -u service_name --no-pager
journalctl -u service_name -o short-iso


### =====================================================================================

# Disk & Storage Management

# Show disk usage of all mounted filesystems
df -h

# Show inode usage
df -i

# Check disk usage of a directory
du -sh /path

# Check disk usage of all folders inside a directory
du -sh /path/*

# Show largest directories (top 20)
du -ah / | sort -h | tail -20

# List block devices (disks, partitions)
lsblk

# Detailed disk information
lsblk -f

# Show partition table
fdisk -l

# Show filesystem UUIDs
blkid

# Mount a device manually
mount /dev/sdXY /mnt

# Unmount a device
umount /mnt

# Mount ISO file
mount -o loop file.iso /mnt

# Create mount directory
mkdir /mnt/mydisk

# Check currently mounted file systems
mount | column -t

# Remount filesystem as read-write
mount -o remount,rw /

# Remount filesystem as read-only
mount -o remount,ro /

# Auto-mount at boot (edit fstab)
vi /etc/fstab

# Example fstab entry
# UUID=<uuid>   /mnt/mydisk   ext4   defaults   0 0

# Check fstab for errors
mount -a

# Check filesystem type
df -T

# Display file/directory disk usage with human readable format
du -h /path

# Display storage usage sorted
du -sh * | sort -h


### ====================================================================================

# Networking Commands — ONE SHEET (ip, ss, netstat, ping, dig)

# ---------------------------
# IP COMMANDS
# ---------------------------

# Show all network interfaces (IP addresses)
ip a

# Show interface details
ip addr show

# Show routing table
ip r

# Add IP address to interface
ip addr add 192.168.1.10/24 dev eth0

# Remove IP address
ip addr del 192.168.1.10/24 dev eth0

# Bring interface up/down
ip link set eth0 up
ip link set eth0 down

# Show link-layer info
ip link

# ---------------------------
# SS COMMANDS (socket status)
# ---------------------------

# Show all listening ports
ss -tulpn

# Show TCP connections
ss -t

# Show UDP connections
ss -u

# Show processes using ports
ss -lptn

# Show listening sockets only
ss -l

# Filter by port
ss -tulpn | grep 80

# ---------------------------
# NETSTAT COMMANDS
# (if available; older tool)
# ---------------------------

# Show all listening ports
netstat -tulpn

# Show active connections
netstat -tn

# Show routing table
netstat -rn

# Show interface stats
netstat -i

# ---------------------------
# PING (connectivity test)
# ---------------------------

# Ping IP or hostname
ping 8.8.8.8
ping google.com

# Ping count 5 times
ping -c 5 8.8.8.8

# ---------------------------
# DIG (DNS lookup)
# ---------------------------

# Simple DNS lookup
dig google.com

# Detailed DNS lookup
dig google.com +trace

# Query only the IP address
dig google.com +short

# Check specific DNS record type
dig google.com MX
dig google.com NS
dig google.com TXT

# Query DNS server manually
dig @8.8.8.8 google.com

# ---------------------------
# TRACE ROUTE
# ---------------------------

# Trace network path to destination
traceroute google.com

# ---------------------------
# HOST COMMAND (simple DNS lookup)
# ---------------------------

host google.com
host -t MX google.com


### +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Cron & Scheduling

# ---------------------------
# View Current User Cron Jobs
# ---------------------------
crontab -l

# Edit Current User Cron Jobs
crontab -e

# Remove Current User Cron Jobs
crontab -r

# ---------------------------
# System-wide Cron Directories
# ---------------------------
/etc/crontab       # main system crontab file
/etc/cron.d/       # directory for custom cron jobs
/etc/cron.hourly/  # scripts run hourly
/etc/cron.daily/   # scripts run daily
/etc/cron.weekly/  # scripts run weekly
/etc/cron.monthly/ # scripts run monthly

# ---------------------------
# Cron Job Format
# ---------------------------
# * * * * * command_to_run
# ┬ ┬ ┬ ┬ ┬
# │ │ │ │ │
# │ │ │ │ └─ day of week (0-7, Sunday=0 or 7)
# │ │ │ └─── month (1-12)
# │ │ └───── day of month (1-31)
# │ └─────── hour (0-23)
# └───────── minute (0-59)

# Example: Run script every day at 3:30 AM
30 3 * * * /path/to/script.sh

# Example: Run every 5 minutes
*/5 * * * * /path/to/script.sh

# Example: Run at 1st of every month at midnight
0 0 1 * * /path/to/script.sh

# ---------------------------
# Run Command Once Using At
# ---------------------------
# Schedule a one-time task
echo "/path/to/script.sh" | at 15:30

# List scheduled at jobs
atq

# Remove a scheduled at job
atrm JOB_ID

# ---------------------------
# Check Cron Logs
# ---------------------------
# Most distributions log cron jobs here:
grep CRON /var/log/syslog          # Debian/Ubuntu
grep CRON /var/log/cron            # CentOS/RHEL

# ---------------------------
# Restart Cron Service
# ---------------------------
systemctl restart cron     # Debian/Ubuntu
systemctl restart crond    # CentOS/RHEL

# Enable cron service at boot
systemctl enable cron      # Debian/Ubuntu
systemctl enable crond     # CentOS/RHEL
