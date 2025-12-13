### Day-9: MariaDB Troubleshooting

MariaDB is a reliable, high-performance, open-source database used to store, manage, and query structured data for applications of any size.
An open-source relational database (RDBMS), forked from MySQL, used to store and manage structured data.

Why use it:
High performance, MySQL-compatible, secure, scalable, and free.

### ======================================================================================================

# MariaDB Troubleshooting

ssh peter@172.16.239.10                        # SSH login to DB server as user 'peter'
# Enter password: Sp!dy, type 'yes' to accept host fingerprint

sudo mysql                                      # Try connecting to MariaDB
# If error: can't connect to local MySQL server

sudo systemctl status mariadb                   # Check MariaDB service status
# Example output: Active: inactive (dead)

sudo systemctl start mariadb                    # Start MariaDB service
# If fails: job for mariadb.service failed...
# Suggests checking logs for details

sudo journalctl -xeu mariadb.service           # Debug MariaDB errors
# Shows why MariaDB failed to start (e.g., missing /var/lib/mysql)

ls /var/lib/mysql                               # Check if database folder exists
sudo ls -lah /var/lib/mysql                     # Check permissions
# May show: No such file or directory

sudo mkdir /var/lib/mysql                        # Create missing MySQL data directory
sudo ls -lah /var/lib/mysql                      # Recheck folder exists

sudo chown -R mysql:mysql /var/lib/mysql        # Set ownership to mysql user/group

sudo systemctl start mariadb                    # Start MariaDB again
sudo systemctl status mariadb                   # Verify service is active

sudo mysql                                      # Connect to MariaDB successfully
show databases;                                  # List all databases
exit                                            # Exit MySQL shell

=================================================================================================

# Explanation of Commands

1. ssh peter@ip → Connect to DB server remotely.

2. sudo mysql → Attempt to connect to MariaDB as root.

3. systemctl status/start mariadb → Check and start MariaDB service.

4. journalctl -xeu mariadb.service → View detailed logs for debugging failures.

5. ls /var/lib/mysql → Check if the MySQL data directory exists.

6. mkdir /var/lib/mysql → Create data directory if missing.

7. chown -R mysql:mysql /var/lib/mysql → Set correct ownership; MariaDB must own its data folder.

8. systemctl start/status mariadb → Restart and verify MariaDB service.

9. mysql → Connect to the database, then show databases; to confirm it works.

### ======================================================================================================

# What is MariaDB?

MariaDB is an open-source relational database management system (RDBMS), created as a fork of MySQL.

a) Stores and manages data in structured tables.

b) Supports SQL queries for creating, reading, updating, and deleting data.

c) Fully compatible with MySQL, but often includes extra features and improvements.

### ======================================================================================================

# Why Use MariaDB?

| Reason                       | Explanation                                            |
| ---------------------------- | ------------------------------------------------------ |
| **Open-source & Free**       | No licensing cost; community-driven development.       |
| **High Performance**         | Optimized for faster queries and storage engines.      |
| **MySQL Compatible**         | Easy migration from MySQL with minimal changes.        |
| **Secure**                   | Regular security updates and features like encryption. |
| **Scalable**                 | Can handle small apps to large enterprise workloads.   |
| **Flexible Storage Engines** | Supports multiple engines like InnoDB, Aria, MyISAM.   |


MariaDB is a reliable, high-performance, open-source database used to store, manage, and query structured data for applications of any size.

### ======================================================================================================