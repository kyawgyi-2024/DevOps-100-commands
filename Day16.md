# Day-16: Install and Nginx as a Load Balancer

(Nginx can be configured as a load balancer using upstream to define backend servers and proxy_pass to route client requests, distributing load across multiple app servers.)

===================================================================================================================

1. Login to Load Balancer Server
ssh loki@stlb01                               # SSH login to LBR server
# Enter password and type 'yes' if prompted

2. Install and Start Nginx
sudo yum install -y nginx                       # Install Nginx web server
sudo systemctl enable --now nginx               # Enable and start Nginx service

3. Check Application Servers (Login to app servers to confirm services are running:)
ssh tony@stapp01                               # Login to first app server
sudo ss -tulnp | grep httpd                     # Check HTTP service port
curl localhost:6300                             # Test service locally
(Test connectivity to other app servers:)

curl stapp01:6300                               # Test from first server
curl stapp02:6300                               # Test from second server
curl stapp03:6300                               # Test from third server
(All application servers should respond on port 6300.)

4. Configure Nginx Load Balancing
sudo vi /etc/nginx/nginx.conf                    # Edit main Nginx configuration

# Example Nginx Load Balancer Config:

include /etc/nginx/conf.d/*.conf;               # under file write config

http {
    upstream stapp {
        server stapp01:6300;
        server stapp02:6300;
        server stapp03:6300;
    }

    server {
        listen 6300;

        location / {
            proxy_pass http://stapp;
        }
    }
}

# Explanation:

a) upstream stapp → Defines backend app servers

b) proxy_pass http://stapp; → Forwards requests to the upstream servers

c) Nginx will load balance requests across stapp01, stapp02, stapp03

Save & exit:
Esc -> :wq -> Enter

5. Test Nginx Configuration
sudo nginx -t                                  # Check Nginx config syntax
sudo nginx -s reload                            # Reload Nginx to apply new config

6. Test Load Balancer Locally
curl localhost:6300                             # Test load balancer locally
curl stapp01:6300                               # Test direct app server

7. Test From Jump Host
# From jumphost
curl stapp01:6300                               # Test application via LB
(Requests should now be distributed among the three app servers.)

============================================================================================================
# Key Concepts (Interview Ready)

Nginx as LBR: Acts as a reverse proxy forwarding requests to multiple backend servers

upstream directive: Defines backend servers

proxy_pass: Sends client requests to upstream servers

Load balancing: Distributes traffic to improve performance & redundancy

curl: Test connectivity and response

================================================================================================================
# What is Nginx?

Nginx (Engine X) is an open-source, high-performance web server that can also act as:

Reverse proxy
Load balancer
HTTP cache
Mail proxy (IMAP/POP3/SMTP)

It is designed to handle thousands of concurrent connections efficiently with low resource usage.

===================================================================================================================
# Why Use Nginx?

Web Server: Serves static content (HTML, CSS, JS) fast.

Reverse Proxy: Forwards client requests to backend application servers (e.g., Tomcat, Node.js).

Load Balancer: Distributes incoming traffic across multiple servers to improve availability and performance.

High Performance: Uses event-driven architecture for handling many concurrent connections.

Security & SSL: Handles HTTPS/SSL termination efficiently.

Caching: Can cache static and dynamic content to reduce backend load.
================================================================================================================
Short Answer:

Nginx is a high-performance web server, reverse proxy, and load balancer used to serve content, distribute traffic, and secure applications.

Mini Version:

Nginx = Web server + reverse proxy + load balancer, high performance and low resource usage.

===============================================================================================================
# What is Nginx Load Balancer?

An Nginx Load Balancer is a configuration of Nginx where it distributes incoming client requests across multiple backend servers (application servers) to improve performance, reliability, and availability.

Nginx acts as a reverse proxy in front of your backend servers.

It ensures that no single backend server becomes a bottleneck.
================================================================================================================
# Why Use Nginx Load Balancer?

1. High Availability
If one backend server fails, traffic is automatically routed to healthy servers.

2. Performance & Scalability
Distributes client requests across multiple servers to handle more traffic efficiently.

3. Redundancy
Prevents downtime by providing multiple servers for the same application.

4. SSL Termination
Nginx can handle HTTPS requests and forward them to backend servers over HTTP, reducing CPU load on app servers.

5. Simplified Maintenance
Servers can be added, removed, or updated without affecting users.
==================================================================================================================
# Key Nginx Load Balancer Features

Round Robin: Default method; distributes requests equally.

Least Connections: Sends requests to server with fewest active connections.

IP Hash: Sends requests from the same client IP to the same server.

Health Checks: Monitors backend servers and removes unhealthy ones automatically.
===============================================================================================================
Short Answer:

Nginx load balancer distributes client traffic across multiple backend servers to improve availability, performance, and reliability.

Mini Version:
Nginx as LBR = traffic distribution + redundancy + high availability for web apps.
=============================================================================================================
