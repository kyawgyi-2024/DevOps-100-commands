## DevOps Day-20: Configure Nginx + PHP-FPM Using Unix Socket

### 🔹 Nginx Installation and Setup

ssh tony@stapp01                    # Login to the app server as user 'tony'

sudo yum install -y nginx           # Install Nginx web server

sudo systemctl enable --now nginx   # Enable Nginx at boot and start the service

sudo systemctl status nginx         # Verify Nginx service is running

### 🔹 Configure Nginx (Port & Document Root)

sudo vi /etc/nginx/nginx.conf       # Open main Nginx configuration file

/Listen 80                          # Search default HTTP port
/Listen 8096                        # (Optional) search for custom port if exists

root /usr/share/nginx/html;         # Default document root
root /var/www/html;                 # Update document root for web applications
Esc :wq                             # Save and exit the file

sudo nginx -t                       # Test Nginx configuration for syntax errors
sudo nginx -s reload                # Reload Nginx configuration without downtime

### 🔹 Install PHP & PHP-FPM (CentOS 9 Stream – PHP 8.2)

sudo yum install -y php php-fpm php-common
# Install PHP and PHP-FPM (FastCGI Process Manager)
sudo systemctl enable --now php-fpm # Enable PHP-FPM at boot and start service

### 🔹 Configure PHP-FPM to Use Unix Socket

sudo vi /etc/php-fpm.d/www.conf     # Open PHP-FPM pool configuration

listen = /run/php-fpm/www.sock
listen = /var/run/php-fpm/default.sock   # Change socket path
Esc :wq                             # Save and exit the file

sudo systemctl restart php-fpm      # Restart PHP-FPM to apply socket changes

curl localhost                      # Initial test (PHP not yet linked with Nginx)

### 🔹 Configure Nginx to Use PHP-FPM Unix Socket
sudo vi /etc/nginx/nginx.conf       # Edit Nginx configuration again

location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/var/run/php-fpm/default.sock;  # Connect via Unix socket
    fastcgi_index index.php;
    include fastcgi.conf;
}
sudo nginx -t                       # Validate Nginx configuration
sudo nginx -s reload                # Reload Nginx to apply PHP configuration

### 🔹 Final Verification
curl localhost                      # Test PHP processing via Nginx + PHP-FPM

sudo systemctl restart nginx        # Restart Nginx if required

✔ Nginx is now successfully serving PHP using PHP-FPM via Unix socket

### 📝 Summary

* Nginx installed and configured
* Document root updated to `/var/www/html`
* PHP-FPM installed and enabled
* PHP-FPM configured to use Unix socket
* Nginx connected to PHP-FPM using Unix socket
* Faster and more secure than TCP-based PHP-FPM

✔ Ideal for DevOps labs, production setups, and interviews
=================================================================================================================