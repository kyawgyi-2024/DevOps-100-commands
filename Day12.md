# Day-12: Linux Network Services (Port, Service & Firewall)

iptables is manual and static; firewalld is dynamic and easier for production use.

# Terminal-1 (Jump Host – Client Side Tests)
curl http://stapp01:6400                      # Test HTTP service on port 6400
telnet stapp01 6400                           # Test TCP connectivity to port 6400
(These may fail initially if service or firewall is not configured.)

# Terminal-2 (Application Server: stapp01)
ssh tony@stapp01                              # Login to app server
# Enter password and accept fingerprint

1. Check Services
sudo systemctl status httpd                  # Check Apache HTTP service status

sudo systemctl status sendmail               # Check Sendmail service
sudo systemctl stop sendmail                 # Stop sendmail service
sudo systemctl disable sendmail              # Disable sendmail at boot
(Sendmail may occupy ports or cause conflicts.)

2. Network & Port Checks
sudo netstat --help                           # View netstat usage options
sudo netstat -tulnp | grep 6400              # Check which service is listening on port 6400

# Flags explanation:
-t → TCP
-u → UDP
-l → Listening ports
-n → Numeric output
-p → Process using the port

3. Start and Enable HTTPD
sudo systemctl start httpd                   # Start Apache HTTP server
sudo systemctl status httpd                  # Verify it is running
sudo systemctl enable httpd                  # Enable Apache at boot

4. Test Locally on App Server
curl http://stapp01:6400                     # Test HTTP service from server

# Terminal-1 (Jump Host – Recheck)
curl http://stapp01:6400                     # Test again from jump host
telnet stapp01 6400                          # Test TCP connection
(Still may fail if firewall blocks the port.)

# Terminal-2 (Firewall Configuration)
5. Check Firewall Rules
sudo iptables --help                         # View iptables help
sudo iptables -L -n --line-numbers           # List firewall rules with line numbers

6. Allow Port 6400 in Firewall
sudo iptables -A INPUT -p tcp --dport 6400 -j ACCEPT
# Append rule to allow TCP traffic on port 6400
(OR insert at a specific position:)
sudo iptables -I INPUT 4 -p tcp --dport 6400 -j ACCEPT
# Insert rule at line number 4

7. Verify Firewall Rules
sudo iptables -L -n --line-numbers            # Recheck firewall rules

8. Final Connectivity Test
curl http://stapp01:6400                     # Verify HTTP access
telnet stapp01 6400                          # Verify port connectivity

# Terminal-1 (Jump Host – Final Check)
curl http://stapp01:6400                     # HTTP check from jump host
telnet stapp01 6400                          # TCP port check from jump host

=================================================================================================

# Key Concepts (Interview Ready)

curl → Test HTTP services
telnet → Test TCP port connectivity
netstat → Check listening ports & services
systemctl → Manage Linux services
iptables → Linux firewall rules
Port 6400 → Custom application/service port

=====================================================================================================
# Linux networking
Linux network services involve service management, port monitoring, and firewall configuration to ensure applications are reachable over the network.

==========================================================================================================

# Why do we use a firewall in Linux?

A firewall is used to:
Control network traffic
Allow required ports/services
Block unauthorized access
Improve server security
In Linux, the most common firewall tools are iptables and firewalld.

============================================================================================================

# What is iptables?
iptables:
iptables is a low-level Linux firewall tool used to manually define rules for allowing or blocking network traffic.

Key points:
Rule-based (INPUT, OUTPUT, FORWARD)
Changes apply immediately
Rules are static (service restart may be needed)
Powerful but complex

Example:
iptables -A INPUT -p tcp --dport 6400 -j ACCEPT   # Allow port 6400

# When to use iptables
Older systems
Simple firewall setups
Deep, manual control over traffic

=================================================================================================================
# What is firewalld?
firewalld:
firewalld is a high-level firewall management service built on top of iptables (or nftables).

Key points:
Zone-based (public, private, trusted, etc.)
Changes can be made without restarting
Easier to manage
Dynamic firewall rules

Example:
firewall-cmd --add-port=6400/tcp --permanent
firewall-cmd --reload

# When to use firewalld
Modern Linux systems (CentOS, RHEL)
Production servers
Frequent firewall changes
===============================================================================================================

# iptables vs firewalld (Comparison Table)

| Feature        | iptables     | firewalld           |
| -------------- | ------------ | ------------------- |
| Level          | Low-level    | High-level          |
| Rule type      | Static rules | Dynamic zones       |
| Ease of use    | Complex      | Easy                |
| Restart needed | Yes          | No                  |
| Production use | Limited      | Recommended         |
| Backend        | Netfilter    | iptables / nftables |

=============================================================================================================

Interview-Friendly Answers:
Short Answer:

# iptables is a low-level firewall tool with static rules, while firewalld is a dynamic, zone-based firewall that is easier to manage.
Why use firewalld over iptables?
Firewalld allows dynamic rule changes without service restart and is better suited for production environments.

Why still use iptables?
For fine-grained control and legacy systems.

====================================================================================================================
# Packet Flow (Very Important for Interviews)

Incoming packet
   ↓
INPUT (to this server)

Outgoing packet
   ↓
OUTPUT (from this server)

Passing through
   ↓
FORWARD (via this server)

========================================================================================================
