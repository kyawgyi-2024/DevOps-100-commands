# Day-14: Linux Process Troubleshooting

(Troubleshooting Linux processes involves checking service status, logs, listening ports, and stopping conflicting services to ensure the application runs correctly.)

1. Login to App Server
ssh steve@ip                                  # SSH login to application server
# Enter password

2. Check Apache HTTP Service
sudo systemctl status httpd                    # Check Apache status
sudo systemctl enable httpd                    # Enable Apache at boot
sudo systemctl start httpd                     # Start Apache service
(Ensures the web server is running and starts automatically after reboot.)

3. Check Apache Logs for Errors
sudo journalctl -xeu httpd                     # View detailed Apache error logs
(Useful for troubleshooting why Apache may fail to start.)

4. Check Listening Ports
sudo ss -tulnp | grep 5000                     # Check if application is listening on port 5000
# ss -tulnp: TCP/UDP listening processes with PID

5. Manage Conflicting Services
sudo systemctl status sendmail                  # Check Sendmail status
sudo systemctl stop sendmail                    # Stop service if it conflicts
sudo systemctl disable sendmail                 # Disable at boot
sudo systemctl status sendmail                  # Verify stopped
(Sometimes Sendmail occupies ports or resources, blocking Apache.)

6. Restart Apache
sudo systemctl start httpd                     # Start Apache again after stopping conflicting services
sudo systemctl status httpd                    # Verify Apache is active

7. Test Locally on App Server
curl localhost:5000                             # Test application locally on port 5000
(Should return the application response)

8. Test Remotely from Jump Host
exit                                           # Exit app server
curl http://stapp01:5000                        # Test application from jump host
(Confirms application is reachable over the network)

==================================================================================================================
# Key Concepts (Interview Ready)

systemctl status/start/enable → Manage services
journalctl -xeu → Debug service errors
ss -tulnp → Check which process is listening on a port
Stop conflicting services → Free ports for app
curl → Test service locally and remotely

===================================================================================================================

