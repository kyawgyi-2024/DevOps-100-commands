## DevOps Day-18: Configure LAMP Server

LAMP is a classic web application stack used to run dynamic websites and web apps, especially on Linux servers.

# What does LAMP stand for?

LAMP =
L → Linux (Operating System)
A → Apache (Web Server)
M → MySQL / MariaDB (Database)
P → PHP (or sometimes Python / Perl)
👉 Together, they let you host and run backend-driven websites.

### 🔹 App Server Configuration

```bash
ssh tony@stapp01                     # Login to app server as user 'tony'
```

```bash
sudo yum install -y httpd php php-mysqlnd php-cli php-common
# Install Apache web server and PHP with MySQL support
```

```bash
curl localhost                       # Test Apache default page (before service start may fail)
```

```bash
sudo systemctl enable --now httpd    # Enable Apache at boot and start service immediately
```

```bash
sudo systemctl status httpd          # Verify Apache service is running
```

```bash
sudo vi /etc/httpd/conf/httpd.conf   # Open Apache main configuration file
```

```text
/Listen 80                           # Search for default listening port
Listen 3000                          # Change Apache port from 80 to 3000
```

```bash
Esc :wq                              # Save and exit the file
```

```bash
sudo systemctl restart httpd         # Restart Apache to apply configuration changes
```

```bash
sudo systemctl status httpd          # Confirm Apache restarted successfully
```

```bash
curl localhost:3000                  # Test Apache on new port (DB connection may fail)
```

> ⚠️ Application cannot connect yet because DB server is not configured

---

### 🔹 Repeat on Other App Servers

```bash
ssh steve@stapp02                    # Login to second app server
ssh banner@stapp03                   # Login to third app server
# Repeat the same Apache + PHP setup steps on all app servers
```

---

### 🔹 Database Server Configuration

```bash
ssh peter@stdb01                     # Login to database server
```

```bash
sudo yum install -y mariadb-server   # Install MariaDB database server
```

```bash
sudo systemctl enable --now mariadb  # Enable MariaDB at boot and start service
```

```bash
sudo systemctl status mariadb        # Verify MariaDB service is running
```

```bash
sudo mysql                           # Login to MariaDB shell as root
```

```sql
CREATE DATABASE db_name;             -- Create application database
SHOW DATABASES;                      -- List all databases

CREATE USER 'db_user'@'%' IDENTIFIED BY 'password';
-- Create DB user accessible from any host

GRANT ALL PRIVILEGES ON db_name.* TO 'db_user'@'%';
-- Grant full access on database to the user

FLUSH PRIVILEGES;                    -- Reload privilege tables

SHOW GRANTS FOR 'db_user'@'%';       -- Verify granted permissions
```

```bash
exit                                 # Exit MariaDB shell
```

---

### 🔹 Final Application Test

```bash
curl localhost:3000                  # Test application from app server
```

✔ Application is now able to connect to the database using `db_user`

---

### 📝 Summary

* Apache + PHP installed on app servers
* Apache port changed from 80 → 3000
* MariaDB installed and configured on DB server
* Database, user, and permissions created
* App successfully connects to DB

✔ Ideal for DevOps labs, interviews, and real-world LAMP setup
===========================================================================================================

# How LAMP works (simple flow)

1. User opens a website in a browser
2. Apache receives the HTTP request
3. If the request needs data:
  a) PHP processes the logic
  b) MySQL stores / fetches data
4. Apache sends the result back to the browser

=============================================================================================================
# Quick Comparison

| Stack           | Used For                |
| --------------- | ----------------------- |
| **LAMP**        | PHP-based apps          |
| **MEAN / MERN** | JavaScript full-stack   |
| **LEMP**        | Nginx instead of Apache |

==============================================================================================================
