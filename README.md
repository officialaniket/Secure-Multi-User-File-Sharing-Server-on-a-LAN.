# Secure-Multi-User-File-Sharing-Server-on-a-LAN.
Designed and implemented a secure, multi-user file  sharing environment on a CentOS/RHEL 7/8 server  tailored for a small office LAN. The system supports  departmental access, enforces file-level security, enables  encrypted file transfers, and ensures maintainability  through process and package management.  

🔐 Secure Multi-User File Sharing Server on a LAN
This project implements a secure file-sharing server that allows multiple users to access, upload, and download files over a Local Area Network (LAN). It supports user authentication, access control, and basic encryption over the network using Samba or OpenSSH (SFTP).

📁 Table of Contents
Features

Requirements

System Architecture

Setup Instructions

User Management

Security Measures

Usage Guide

Screenshots

Future Improvements

License

✅ Features
Secure file sharing via LAN

Multi-user access with login authentication

Role-based permissions (read/write)

Encrypted transmission using SFTP

Logging and file activity tracking

Simple web-based or CLI interface (optional)

🔧 Requirements
OS: Ubuntu 20.04+ / Debian / CentOS / RHEL

Packages:

openssh-server

samba (optional)

python3 (for optional scripts)

ufw or firewalld for firewall rules

🧱 System Architecture
Copy
Edit
+------------------+         +---------------------+
|   User Device 1  |  <--->  |                     |
|   User Device 2  |  <--->  |  File Server (LAN)  |
|   User Device N  |  <--->  |                     |
+------------------+         +---------------------+
                                |
                          [User Database]
                                |
                          [Encrypted Disk]
🛠️ Setup Instructions
1. Update System
bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
2. Install OpenSSH Server (SFTP)
bash
Copy
Edit
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
3. Create File Sharing Directory
bash
Copy
Edit
sudo mkdir -p /srv/fileshare
sudo chown root:root /srv/fileshare
sudo chmod 755 /srv/fileshare
4. Create User Groups
bash
Copy
Edit
sudo groupadd fileshare_users
5. Add Users
bash
Copy
Edit
sudo useradd -m -s /bin/bash -G fileshare_users username
sudo passwd username
6. Restrict Users to SFTP
Edit /etc/ssh/sshd_config:

bash
Copy
Edit
Match Group fileshare_users
    ChrootDirectory /srv/fileshare
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
Then restart SSH:

bash
Copy
Edit
sudo systemctl restart ssh
7. Set Permissions
bash
Copy
Edit
sudo chown root:root /srv/fileshare
sudo mkdir /srv/fileshare/uploads
sudo chown :fileshare_users /srv/fileshare/uploads
sudo chmod 770 /srv/fileshare/uploads
👥 User Management
Add user:

bash
Copy
Edit
sudo useradd -m -s /bin/bash -G fileshare_users newuser
sudo passwd newuser
Remove user:

bash
Copy
Edit
sudo userdel -r username
Change permissions:

bash
Copy
Edit
sudo chmod 770 /srv/fileshare/uploads
🔐 Security Measures
🔒 Encrypted transfer with SFTP

🛡️ UFW firewall enabled:

bash
Copy
Edit
sudo ufw allow OpenSSH
sudo ufw enable
📜 User isolation via chroot jail

🚫 Disable root login and password auth (optional hardening)

📚 Usage Guide
Connect to server from client using:

Copy
Edit
sftp username@<server-ip>
Upload a file:

Copy
Edit
put myfile.txt
Download a file:

Copy
Edit
get report.pdf
List files:

Copy
Edit
ls
