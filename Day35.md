# DevOps-Day 35 – Install Docker & Docker Compose

🎯 Goal
Install Docker Engine (docker-ce) and Docker Compose
Start & enable Docker service
Allow a normal user (steve) to run Docker without sudo
Verify installation

1️⃣ Install Required DNF Plugin
---
sudo dnf -y install dnf-plugins-core

Why?
Adds dnf config-manager
Needed to add external repositories (like Docker’s repo)

2️⃣ Add Official Docker Repository
---
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

Why?
Docker packages are not available by default
This repo provides:
docker-ce
docker-compose plugin
containerd

3️⃣ Install Docker Packages
---
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Packages Explained:
Package	Purpose
docker-ce	Docker Engine
docker-ce-cli	Docker command-line
containerd.io	Container runtime
docker-buildx-plugin	Advanced image builds
docker-compose-plugin	docker compose command

4️⃣ Start & Enable Docker Service
---
sudo systemctl enable --now docker

Why?
enable → start Docker on boot
--now → start Docker immediately

5️⃣ Check Docker Service Status
---
sudo systemctl status docker


✅ Confirms Docker is:
Active
Running without errors

6️⃣ Verify Docker Is Working
---
docker ps

Output meaning:
Shows running containers (empty list = Docker OK)

7️⃣ Allow Non-Root User to Run Docker
---
sudo usermod -aG docker steve

Why?
Adds steve to docker group
Allows Docker commands without sudo
⚠️ User must log out & log in again for this to take effect

8️⃣ Verify Docker as Normal User
---
docker ps
✔ Confirms user permission works

9️⃣ Verify Docker & Compose Version
---
docker --version
docker compose version

Notes:
❌ docker composer → WRONG
✅ docker compose → CORRECT (new syntax)

✅ Final Verification Checklist
✔ Docker installed
✔ Docker service running
✔ Docker starts on boot
✔ Docker Compose available
✔ Non-root user access enabled

# 📝 Quick Interview Tip
Q: Why Docker Compose plugin instead of old docker-compose?
A: Newer Docker versions integrate Compose directly as
docker compose, improving performance and compatibility.