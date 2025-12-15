# Day-15: Setup SSL for Nginx

SSL is a security protocol that encrypts data between client and server, ensuring confidentiality, integrity, and authentication.
SSL = Encrypted, secure communication between browser and server (HTTPS).
SSL (Secure Sockets Layer) is a security protocol.
(Setting up SSL for Nginx involves installing Nginx, securing SSL certificates, configuring server blocks, and verifying HTTPS access both locally and remotely.)

================================================================================================================

1. Login to Server
ssh name@ip                                   # Login to app server
ssh banner@stapp03                            # Login to Nginx server
# Enter passwords and type 'yes' if prompted

2. Install and Start Nginx
sudo yum install -y nginx                     # Install Nginx web server
sudo systemctl enable --now nginx             # Enable and start Nginx service
sudo systemctl status nginx                   # Verify Nginx is running

3. Test Nginx
curl localhost                               # Test HTTP (port 80)
curl https://localhost                        # Test HTTPS (port 443) – may fail before SSL config

4. Prepare SSL Directory & Certificates
ls -lah /etc/nginx/                           # Check Nginx config folder
sudo mkdir /etc/nginx/ssl                     # Create folder for SSL files
sudo mv /tmp/nautilus.* /etc/nginx/ssl       # Move SSL cert/key to SSL folder
ls -lah /etc/nginx/ssl                        # Verify files
sudo chmod 600 /etc/nginx/ssl/nautilus.key   # Secure private key
ls -lah /etc/nginx/ssl                        # Verify permissions

5. Create Nginx SSL Configuration
sudo touch /etc/nginx/conf.d/nautilus.conf    # Create new SSL config file
sudo vi /etc/nginx/conf.d/nautilus.conf       # Edit SSL config

Example config:

server {
    listen [::]:443 ssl http2;
    server_name _;
    root /usr/share/nginx/html;

    ssl_certificate "/etc/nginx/ssl/nautilus.crt";
    ssl_certificate_key "/etc/nginx/ssl/nautilus.key";

    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}

(Make sure file paths and names are correct (nautilus.key not nautilu.key))
Save & exit: Esc -> :wq -> Enter

6. Test Nginx Configuration
sudo nginx -t                                # Test Nginx config syntax
sudo nginx -s reload                          # Reload Nginx
sudo systemctl restart nginx                  # Restart service
sudo systemctl status nginx                   # Verify running

7. Test HTTPS Locally
curl https://localhost                        # Test HTTPS (may warn for self-signed)
curl --insecure https://localhost             # Ignore certificate warnings for testing

8. Create Test Web Page
sudo vi /usr/share/nginx/html/index.html
# Insert: Welcome!
Esc -> :wq -> Enter
curl --insecure https://localhost             # Test page content

9. Test From Jump Host
exit                                         # Exit Nginx server
curl --insecure https://stapp02              # Test HTTPS from jump host

=================================================================================================================
# Key Concepts (Interview Ready)

ssl_certificate & ssl_certificate_key → Define SSL cert and key paths
chmod 600 → Secure private key
nginx -t → Check configuration syntax
nginx -s reload → Apply config without downtime
curl --insecure → Test self-signed certificates

===================================================================================================================
# What is SSL?

SSL (Secure Sockets Layer) is a security protocol that encrypts data transmitted between a client (browser) and a server (web/application server).

Modern systems often use TLS (Transport Layer Security), which is an updated version of SSL.

You’ll see it as HTTPS in browsers (https://).
===================================================================================================================
# Why Use SSL?

1. Encrypt Data
Protects sensitive information (passwords, credit cards, API data) from attackers.

2. Authentication
Confirms that the server you are connecting to is legitimate.

3. Data Integrity
Ensures that data is not altered during transit.

4. Trust & Compliance
Required for PCI DSS, GDPR, and other regulations.
Users trust websites with HTTPS more than HTTP.

===================================================================================================================