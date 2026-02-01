# Devops-nginx-docker

🚀 EC2 Server Setup with Nginx, Docker & Logs

This repository documents the basic setup of an AWS EC2 Ubuntu instance including:

System update

Nginx installation and verification

Docker installation and user configuration

Security Group configuration (port 80)

Service logging using journalctl

Downloading logs to a local machine

📌 Prerequisites

AWS EC2 Ubuntu instance

SSH access using .pem key

Port 22 (SSH) open in Security Group

1️⃣ Update System Packages

Update the package index to ensure the latest versions are available.

sudo apt update

2️⃣ Install and Verify Nginx
Install Nginx
sudo apt install nginx

Check Nginx Status
systemctl status nginx


Expected output:

active (running)

3️⃣ Install and Verify Docker
Install Docker
sudo apt install docker

Check Docker Status
systemctl status docker


Docker service should be running successfully.

4️⃣ Add User to Docker Group

By default, Docker requires sudo. Adding the user to the Docker group removes this requirement.

sudo usermod -aG docker $USER


Apply group changes:

newgrp docker


Alternatively, log out and log in again.

Verify Docker Access
docker ps


If no permission error appears, Docker is configured correctly.

5️⃣ Open Port 80 in EC2 Security Group

To access Nginx from a browser, port 80 must be open.

Steps:

AWS Console → EC2 → Instances

Select your instance

Go to Security tab

Open attached Security Group

Edit Inbound Rules

Add Rule:
Type	Protocol	Port	Source
HTTP	TCP	80	0.0.0.0/0

⚠️ For production, restrict source IPs.

6️⃣ Save Nginx Logs to a File

Use journalctl to export Nginx logs into a file.

journalctl -u nginx > logs.txt


This creates a log file in the current directory.

7️⃣ Download Logs to Local Machine

Use scp to download logs from EC2 to your local system.

scp -i your-key.pem ubuntu@<your-instance-ip>:logs.txt .

Example:
scp -i mykey.pem ubuntu@13.xxx.xxx.xxx:logs.txt .


The file will be downloaded to the current local directory.

🔐 Notes & Tips

Default Ubuntu EC2 user: ubuntu

Ensure SSH key permissions:

chmod 400 your-key.pem


Port 22 (SSH) must be open

Logs may require sudo before copying to home directory
