<h1 align="center">
  <img src="https://github.com/senthilpoo10/badges/blob/main/badges/born2berootm.png" width="250"/>
</h1>

<p align="center">
  <b><i>Born2beRoot: Secure Server Configuration</i></b><br>
  <i>"Mastering Linux system administration with strict security policies"</i>
</p>

<p align="center">
  <img alt="score" src="https://img.shields.io/badge/score-100%2F100-brightgreen" />
  <img alt="difficulty" src="https://img.shields.io/badge/difficulty-★★★★☆-orange" />
  <img alt="time spent" src="https://img.shields.io/badge/time%20spent-50+%20hours-blue" />
</p>

## 📚 Project Overview

Born2beRoot is a system administration project that teaches secure server configuration on either Debian or Rocky Linux. The project focuses on:
- Partitioning with LVM and encryption
- Strict password policies
- Sudo security hardening
- Firewall configuration
- Automated monitoring

## 🛠️ System Requirements

| Component | Specification |
|-----------|---------------|
| Virtualization | VirtualBox ≥6.1 or UTM (Mac M1) |
| OS Version | Debian 11 (stable) or Rocky Linux 8 |
| Disk Space | Minimum 8GB (20GB recommended) |
| RAM | 1024MB minimum |

## 🔧 Mandatory Setup Steps

### 1. Virtual Machine Creation
```bash
# For Debian:
wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.x.x-amd64-netinst.iso

# For Rocky:
wget https://download.rockylinux.org/pub/rocky/8/isos/x86_64/Rocky-8.x-x86_64-minimal.iso
```

### 2. Partitioning Scheme
```
sda
├─sda1     /boot       500MB
└─sda2
   └─sda5_crypt (LUKS encrypted)
      ├─vg-root    /        10GB
      ├─vg-swap   [SWAP]    2GB
      └─vg-home   /home     5GB
```

### 3. Essential Configuration
```bash
# Set hostname
hostnamectl set-hostname login42

# Configure SSH (port 4242)
sudo apt install openssh-server # Debian
sudo dnf install openssh-server # Rocky
sudo nano /etc/ssh/sshd_config
```

## 🔒 Security Policies

### Password Requirements
| Policy | Setting |
|--------|---------|
| Expiration | 30 days |
| Minimum age | 2 days |
| Warning period | 7 days |
| Length | ≥10 chars |
| Complexity | Upper+lower+number |
| Consecutive chars | ≤3 identical |

### Sudo Configuration
```bash
# /etc/sudoers.d/secure
Defaults passwd_tries=3
Defaults badpass_message="Authentication failed!"
Defaults logfile="/var/log/sudo/sudo.log"
Defaults requiretty
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

## 📊 Monitoring Script (monitoring.sh)

**Required Output:**
1. System architecture and kernel version
2. CPU information (physical/virtual)
3. Memory usage (RAM)
4. Disk utilization
5. CPU load percentage
6. Last reboot time
7. LVM status
8. Active connections
9. Logged-in users
10. Network information (IP/MAC)
11. Sudo command count

**Implementation Tip:**
```bash
#!/bin/bash
# Architecture
arch=$(uname -a)
# CPU physical
cpu_p=$(grep "physical id" /proc/cpuinfo | wc -l)
# [... other commands ...]
wall -n "#Architecture: $arch
#CPU physical: $cpu_p
[...]"
```

## 🧪 Testing Protocol

1. Verify SSH access:
```bash
ssh login@localhost -p 4242
```

2. Check password policies:
```bash
sudo chage -l login
```

3. Test sudo logging:
```bash
sudo -k # Clear cached credentials
sudo ls # Enter wrong password 3 times
cat /var/log/sudo/sudo.log
```

4. Verify firewall:
```bash
sudo ufw status # Debian
sudo firewall-cmd --list-all # Rocky
```

## 📝 Peer Evaluations (3/3)

> **Evaluation 1**: "Poonkodi's work was very well done she was well prepared for the defense and she was able to explain in good details her reasons and her methods and terminologies. Thanks Poonkodi and well done."

> **Evaluation 2**: "Everything worked as it should and Poonkodi was able to answers all my questions. She remembered the commands very well and seemed knowledgeable about the subject. Good job!"

> **Evaluation 3**: "Very good project, Poonkodi was able to explain and talk about every topic of a project very in detain. everything was working according to subject and I learnt also from you some new command. Good luck to you with your further projects!"

## ⚠️ Common Pitfalls

1. **Partitioning Errors**
   - Forgetting to encrypt LVM partitions
   - Incorrect mount points

2. **Security Misconfigurations**
   - SSH root login not disabled
   - Firewall not enabled at boot

3. **Script Issues**
   - Monitoring script not running automatically
   - Visible error messages in output

## 🚀 Bonus Implementation

### Recommended Bonus Services:
1. **WordPress Stack**
   - lighttpd web server
   - MariaDB database
   - PHP-FPM

2. **Useful Services**
   - Cockpit (web-based admin)
   - Fail2ban (security)
   - Postfix (mail server)

### Bonus Partitioning Example:
```
LVMGroup
├─root    /        10GB
├─swap   [SWAP]    2GB
├─home   /home     5GB
├─var    /var      3GB
├─srv    /srv      3GB
└─tmp    /tmp      3GB
```

## 📌 Submission Requirements

1. Create `signature.txt` with VM disk checksum:
```bash
# For VirtualBox (Linux/Mac):
shasum ~/VirtualBox\ VMs/Born2beRoot/Born2beRoot.vdi > signature.txt

# For UTM (Mac M1):
shasum ~/Library/Containers/com.utmapp.UTM/Data/Documents/Born2beRoot.utm/Images/disk-0.qcow2 > signature.txt
```

2. **Important**: Never commit the actual VM files to Git!
