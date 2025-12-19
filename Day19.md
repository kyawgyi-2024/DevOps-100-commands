## DevOps Day-19: Install and Configure Web Application

### 🔹 Check Application Files (Jump Server)

ls -lah                             # List all files and directories with details

ls -lah blog/                       # View contents and permissions of blog application

ls -lah demo/                       # View contents and permissions of demo application

cd blog/                            # Move into blog directory
cat index.html                      # View blog application's main HTML file
---

### 🔹 Copy Application Files to App Server

scp -r blog/ tony@stapp01:~/        # Copy blog app directory to app server (stapp01)

scp -r demo/ tony@stapp01:~/        # Copy demo app directory to app server (stapp01)

---

### 🔹 Web Server Setup on App Server (stapp01)

ssh tony@stapp01                    # Login to app server

sudo systemctl enable httpd         # Enable Apache to start on boot

sudo systemctl start httpd          # Start Apache service

sudo systemctl status httpd         # Verify Apache service status

curl localhost                      # Test Apache default page on port 80

### 🔹 Change Apache Port Configuration

sudo vi /etc/httpd/conf/httpd.conf  # Open Apache configuration file

/Listen 80                          # Search for default listening port
Listen 3000                         # Change Apache port from 80 to 3000
Esc :wq                             # Save and exit the file

sudo systemctl restart httpd        # Restart Apache to apply port changes

curl localhost:3000                 # Recheck Apache on new port
---

### 🔹 Deploy Web Applications

ls -lah /var/www/html               # Check default Apache web root directory
sudo mv blog/ demo/ /var/www/html   # Move application directories into web root
ls /var/www/html                    # Verify blog and demo directories exist

### 🔹 Final Application Test

curl http://localhost:3000/blog/    # Test blog application

curl http://localhost:3000/demo/    # Test demo application

✔ Both web applications are now accessible via Apache on port 3000

---

### 📝 Summary

* Application files prepared on jump server
* Files securely copied using SCP
* Apache web server started and configured
* Port changed from 80 → 3000
* Web applications deployed to `/var/www/html`
* Applications successfully accessible via browser/curl

✔ Suitable for DevOps labs, real deployments, and interviews
