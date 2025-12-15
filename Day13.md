# Day-13: iptables Installation & Configuration

1. Login to Server
ssh tony@stapp01                               # SSH login to app server
# Enter password for user tony

2. Install iptables Services
sudo yum install -y iptables-services          # Install iptables service package
sudo systemctl enable --now iptables           # Enable iptables at boot and start service
sudo systemctl status iptables                 # Verify iptables service is running

3. Flush Existing Rules (Fresh Start)
sudo iptables -F                               # Flush (delete) all existing rules

4. Add Basic Required Rules
sudo iptables -A INPUT -i lo -j ACCEPT         # Allow loopback (localhost) traffic
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# Allow established and related connections

5. Allow Required Ports
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# Allow SSH access
sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 8089 -j ACCEPT
# Allow port 8089 only from specific IP

6. Block Port for Everyone Else
sudo iptables -A INPUT -p tcp --dport 8089 -j DROP
# Drop all other traffic to port 8089

7. Verify Firewall Rules
sudo iptables -L -n --line-numbers             # List rules with line numbers

8. Save iptables Rules (Important!)
sudo service iptables save                     # Save firewall rules permanently
# Enter password for tony if prompted

9. Restart iptables Service
sudo systemctl restart iptables                # Restart iptables service

10. Recheck Rules After Restart
sudo iptables -L -n --line-numbers             # Verify rules persist after restart

========================================================================================================
# What This Configuration Does

Allows localhost traffic
Allows existing connections
Allows SSH (port 22) from anywhere
Allows port 8089 only from IP 172.16.238.14
Blocks port 8089 from all other IPs
Makes rules persistent across reboots

Installed iptables, configured secure INPUT rules, restricted port access by IP, and saved rules for persistence.
============================================================================================================
# Why Loopback & ESTABLISHED Rules Are Critical
iptables -A INPUT -i lo -j ACCEPT

Purpose: Allows traffic on the loopback interface (lo), i.e., 127.0.0.1.

Why critical:
Many services on Linux communicate internally via loopback.
Blocking it can break local applications, like databases, web servers, and system daemons.

=============================================================================================================
# ESTABLISHED/RELATED Rule
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
Purpose: Allows return traffic for connections that were initiated by the server or are related to existing connections.

Why critical:
Prevents breaking ongoing connections (SSH, HTTP responses).
Keeps firewall rules simple without explicitly allowing every outbound port.

=============================================================================================================
# Difference Between DROP vs REJECT

| Feature            | DROP                               | REJECT                                        |
| ------------------ | ---------------------------------- | --------------------------------------------- |
| Behavior           | Silently discards packets          | Actively refuses packets and sends error back |
| Response to sender | No response                        | Sends ICMP “port unreachable”                 |
| Security           | More secure (stealth)              | Less stealth, reveals port exists             |
| Use case           | Block unknown or malicious traffic | Block but notify trusted clients              |

==================================================================================================================
iptables -A INPUT -p tcp --dport 8089 -j DROP
# Blocks all traffic silently

iptables -A INPUT -p tcp --dport 8089 -j REJECT
# Blocks but tells sender port is closed
(In production, DROP is preferred for unknown/malicious traffic to hide open ports.)
==================================================================================================================
Interview One-Liners

1. Loopback & ESTABLISHED rules: Ensure internal traffic and ongoing connections aren’t blocked.

2. DROP vs REJECT: DROP is silent and stealthy; REJECT sends a response.

3. Real-world iptables: Restrict SSH, web, and database access; limit connections to prevent DoS; allow only trusted IPs.

===================================================================================================================


