# DevOps-Day-36: Deploy Nginx Container on Application Server
# Q.
The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 3. Complete the task with the following instructions:
On Application Server 3 create a container named nginx_3 using the nginx image with the alpine tag. Ensure container is in a running state.

# 1️⃣ Create and run the nginx container
docker run -d --name nginx_3 nginx:alpine

-d → runs the container in detached mode
--name nginx_3 → names the container
nginx:alpine → uses the nginx image with the alpine tag

# 2️⃣ Verify the container is running
docker ps

You should see nginx_3 listed with status Up ✅
That’s it — the nginx container is created and running on Application Server 3.