# Day 37: Copy File to Docker Container | 100 Days of DevOps
# Q.
The Nautilus DevOps team possesses confidential data on App Server 1 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.
Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /usr/src/. Ensure the file is not modified during this operation.
---------------------------------------------------------------------------------------------------
To copy the encrypted file without modifying it, use Docker’s built-in docker cp command. This performs a byte-for-byte copy.

# ✔️ Verified the file exists on the host
ls /tmp/nautilus.txt.gpg

# Command to run on App Server 1 (Docker host)
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src/

# If Docker requires root privileges
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src/

# (Optional) Verify inside the container
docker exec -it ubuntu_latest ls -l /usr/src/nautilus.txt.gpg

✅ This copies /tmp/nautilus.txt.gpg from the host to /usr/src/ inside the ubuntu_latest container without altering the file.
---------------------------------------------------------------------------------------------------

[tony@stapp01 ~]$ history
    1  docker ps
    2  ls /tmp/nautilus.txt.gpg
    3  docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src/
    4  docker exec -it ubuntu_latest ls /usr/src/
    5  docker exec -it ubuntu_latest ls -lh /usr/src/
[tony@stapp01 ~]$ 

✅ Task completed successfully
✔️ Copied the encrypted file to the container without modification
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src/
✔️ Verified the file is present inside the container
docker exec -it ubuntu_latest ls -lh /usr/src/