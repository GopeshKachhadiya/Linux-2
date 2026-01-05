# Linux-2

## Project Overview

Linux Lab 2 is part of a multi-machine vulnerable lab environment designed to simulate a small enterprise network for offensive security training.  
This lab represents an internally accessible Linux server exposing an FTP service configured with weak credentials, allowing an attacker to gain initial access through brute-force attacks.

Post-compromise enumeration reveals a sudo misconfiguration that enables privilege escalation to the root user.

The lab is designed to follow a black-box penetration testing methodology and focuses on common real-world weaknesses found in legacy or poorly managed internal servers.

---

## Lab Objectives

- Identify and enumerate FTP services
- Perform FTP brute-force attacks
- Gain authenticated access to the system
- Perform post-exploitation enumeration
- Identify sudo misconfigurations
- Escalate privileges to root
- Capture user and root flags

---

## Vulnerability Summary

- **Attack Type:** Credential Brute Force
- **Initial Access Vector:** FTP service
- **Privilege Escalation Vector:** Misconfigured sudo permissions
- **Testing Methodology:** Black-box penetration testing

---

## Impact Description

Weak authentication on the FTP service allows an attacker to gain unauthorized access to a valid system account.  
Once logged in, improper sudo configuration enables the attacker to execute privileged commands without authentication, resulting in full system compromise.

### Security Impact
- Unauthorized user access
- Privilege escalation to root
- Full system takeover
- Increased risk of lateral movement within the network

---

## Exploitable Operating System

- **Target OS:** Ubuntu Server (CLI-only)
- **Architecture:** x86_64
- **Hardening Level:** Minimal (intentional misconfiguration)

---

## Exploited Service Details

- **Service Name:** vsftpd
- **Port:** 21/TCP
- **Authentication Method:** Local user authentication
- **Misconfiguration:**
  - Weak FTP credentials
  - FTP service accessible internally
  - Sudo misconfiguration allowing privilege escalation

---

## Attack Flow

- Network scanning and service discovery
- FTP service enumeration
- Credential brute-force attack against FTP
- Successful login using weak credentials
- Access to user-level shell
- Enumeration of sudo permissions
- Privilege escalation to root
- Flag capture

---

## Flags

- **User Flag Location:** `/home/ftpuser/user.txt`
- **Root Flag Location:** `/root/root.txt`

---

## Minimum System Requirements

### Attacker Machine
- Kali Linux (recommended)
- 2 GB RAM
- 1 CPU core
- 20 GB disk space
- Tools:
  - Nmap
  - Hydra
  - FTP client
  - SSH client

### Vulnerable Machine
- Ubuntu Server
- 2 GB RAM
- 1 CPU core
- 20 GB disk space
- VMware Workstation or VirtualBox

### Network
- Host-only or internal network
- Reachable from attacker or via pivoting

---

## Lab Creation Steps (Reproducible)

Follow the steps below to recreate the Linux Lab 2 machine.

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install FTP service
sudo apt install vsftpd -y
sudo systemctl enable vsftpd
sudo systemctl start vsftpd

# Configure vsftpd
sudo nano /etc/vsftpd.conf

listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=NO

sudo systemctl restart vsftpd

# Create FTP user with weak password
sudo useradd -m ftpuser
sudo passwd ftpuser

# Create user flag
echo "user_flag{ftp_bruteforce_success}" | sudo tee /home/ftpuser/user.txt
sudo chown ftpuser:ftpuser /home/ftpuser/user.txt
sudo chmod 644 /home/ftpuser/user.txt

# Create root flag
sudo bash -c 'echo "root_flag{sudo_misconfiguration}" > /root/root.txt'
sudo chmod 600 /root/root.txt

# Introduce privilege escalation vulnerability
sudo visudo

# Add the following line
ftpuser ALL=(ALL) NOPASSWD: /bin/bash
```
## Learning Outcomes

- Understanding FTP service security weaknesses

- Practical experience with credential brute-force attacks

- Importance of strong authentication policies

- Identifying and exploiting sudo misconfigurations

- Linux privilege escalation fundamentals

- Internal network attack simulation

## Disclaimer

This lab is intended strictly for educational and ethical penetration testing purposes.
Do not deploy these configurations in production environments.

## Author

Created BY Gopesh Kachhadiya
This lab was created as part of an offensive cybersecurity internship project focused on realistic enterprise attack simulations and hands-on learning.
