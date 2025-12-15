### Day-11: Install and Configure Tomcat Server

# What is Tomcat?
Apache Tomcat is an open-source Java web application server used to run Java-based web applications (Servlets & JSP).

1. Login to Application Server

ssh banner@stapp03                             # SSH login to app server stapp03
# Type 'yes' to accept fingerprint and enter password
exit                                          # Exit back to jumphost

2. Copy Application WAR File from Jumphost

scp /tmp/ROOT.war banner@stapp03:~/            # Copy ROOT.war from jumphost to app server home directory
# scp = secure copy (source → destination)

3. Login Again and Verify File

ssh banner@stapp03                             # Login to app server again
ls                                             # Verify ROOT.war file exists

4. Install Tomcat

sudo yum install -y tomcat                     # Install Tomcat web server

5. Change Tomcat Port

sudo vi /etc/tomcat/server.xml                 # Edit Tomcat configuration file
<Connector port="8080" ... />
8080 → 5003                                    # Change default Tomcat port
Esc :wq                                       # Save and exit vi editor

6. Start and Enable Tomcat

sudo systemctl status tomcat                   # Check Tomcat service status
sudo systemctl enable --now tomcat             # Enable Tomcat at boot and start service
sudo systemctl status tomcat                   # Recheck Tomcat is running

7. Test Tomcat Access

curl localhost:5003                            # Test Tomcat locally on app server
curl stapp03:5003                              # Test using hostname

8. Check Tomcat Directories

ls -lah /usr/share/tomcat/                     # View Tomcat installation directory
ls -lah /usr/share/tomcat/webapps              # Check deployed web applications

9. Deploy Application (ROOT.war)

sudo mv ROOT.war /usr/share/tomcat/webapps/    # Move application WAR file to Tomcat webapps directory
# mv = move file from source to destination

# Verify deployment:
ls -lah /usr/share/tomcat/webapps              # Check ROOT.war or ROOT directory
ls -lah /usr/share/tomcat/webapps/ROOT         # Verify extracted ROOT application

10. Recheck Application Access

curl stapp03:5003                              # Verify application is running on app server
exit                                          # Exit app server
curl stapp03:5003                              # Verify from jumphost

### ========================================================================================================

# Key Concepts (Interview Ready)

1. Tomcat → Java web application server

2. WAR file → Web Application Archive

3. webapps/ → Deployment directory

4. server.xml → Port configuration

5. curl → Test web application without browser

### ========================================================================================================

# What is Tomcat?
Apache Tomcat is an open-source Java web application server used to run Java-based web applications (Servlets & JSP).

# Why use Tomcat?
Lightweight and easy to configure
Free and open-source
Widely used for Java web apps
Supports WAR file deployment
Integrates well with CI/CD pipelines

# Super Short Interview Answer : 
Tomcat is a lightweight Java application server used to deploy and run Java web applications packaged as WAR files.

========================================================================================================
# Difference Between WAR vs JAR

| Feature    | WAR                         | JAR                           |
| ---------- | --------------------------- | ----------------------------- |
| Full form  | Web Application Archive     | Java Archive                  |
| Used for   | Web applications            | Java applications / libraries |
| Runs on    | Application server (Tomcat) | JVM directly                  |
| Contains   | JSP, Servlets, HTML, config | Java classes & metadata       |
| Deployment | Placed in `webapps/`        | Run using `java -jar`         |

========================================================================================================

java -jar app.jar             # Run standalone Java application
cp app.war /usr/share/tomcat/webapps/   # Deploy web app on Tomcat

WAR is for Java web apps deployed on servers like Tomcat, while JAR is for standalone Java applications.

============================================================================================================
# Tomcat Real DevOps Deployment Flow (Production Level)

Developer
  ↓
Git Repository (GitHub/GitLab)
  ↓
CI Tool (Jenkins)
  ↓
Build Tool (Maven/Gradle)
  ↓
Generate WAR file
  ↓
Artifact Storage (Nexus / S3)
  ↓
Application Server (Tomcat)

=============================================================================================================
# Simple Deployment Commands (Example)

scp app.war banner@stapp03:/usr/share/tomcat/webapps/   # Deploy WAR
systemctl restart tomcat                                # Restart Tomcat
curl stapp03:5003                                       # Verify app

============================================================================================================
# Final Interview Summary (Perfect Answer)

Tomcat runs Java web applications.

WAR is deployed on Tomcat, JAR runs directly on JVM.

DevOps flow: Git → Jenkins → Maven → WAR → Tomcat.

================================================================================================================
