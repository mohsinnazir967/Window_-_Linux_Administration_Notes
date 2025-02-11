
# Task 1: Web Server Setup & Automation  

## Objective:

Set up a Nginx web server with automated deployment of a static website.

## Task

**Install Nginx and ensure it starts on boot.**

```
sudo apt install nginx
```

```
systemctl start nginx
```

```
systemctl enable nginx
```

**Create a directory /var/www/lab-site and add a sample index.html with the text "Welcome to Intern Lab".**

```
cd /var/www/
```

```
mkdir lab-site
```

```
cd lab-site
```

```
sudo touch index.html
```

```
echo "Welcome to Intern Lab" | sudo tee index.html
``` 

**Configure Nginx to serve the site from /var/www/lab-site on port 8080.**

navigate to `/etc/nginx/sites-available` and create a configuration file for our site.

```
cd /etc/nginx/sites-available
```

```
sudo touch lab-site
```

Edit the lab-site configuration and add these lines

```
sudo vim lab-site
```

add this configuraiton

```
# Virtual Host configuration for example.com

# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.

server {
	listen 8080;
	listen [::]:8080;

	server_name labsite.com;

	root /var/www/lab-site;
	index index.html;

	location / {
		try_files $uri $uri/ =404;
	}
}
```

Create a link of this configuration file in the `sites-enabled` directory

```
sudo ln -s /etc/nginx/sites-available/lab-site /etc/nginx/sites-enabled/lab-site
```

Open firebox and browser to `localhost:8080` the webpage will display

**Write a script (deploy.sh) to automate copying files from /home/user/web-files to /var/www/lab-site and reload Nginx.**

```
touch deploy.sh
```

```
sudo vim deploy.sh
```

Add this lines and save the files

```
#!/bin/bash

#script (deploy.sh) to automate copying files from /home/user/web-files to /var/www/lab-site and reload Nginx

# Copying Files
#
sudo cp /home/mohsin/webfiles/* /var/www/lab-site.

# Reloading Nginx

systemctl restart nginx

```

**Use cron to run the script every day at 2 AM.**

```
crontab -e
```

Add this entry, save and quit.

```
0 2 * * * /home/mohsin/deploy.sh
```

# Network Monitoring & Log Analysis 

## Objective: 

Monitor network traffic and analyze logs for suspicious activity.  

## Tasks:

Use tcpdump to capture 100 packets on interface eth0 and save to capture.pcap.

```
sudo tcpdump -i enp3s0 -c 100 -w capture.pcap
```

Install iftop to monitor real-time bandwidth usage.

```
sudo apt install iftop
```

```
sudo iftop
```


**Write a script (scan-logs.sh) to search /var/log/auth.log for "Failed password" attempts and save results to failed-logins.txt.**

```
touch scan-logs.sh
```

```
vim scan-logs.sh
```

Give permissions

```
chmod 777 scan-logs.shls
```

**Block an IP address (e.g., 192.168.1.100) with ufw if it appears in failed-logins.txt.**

```
#!/bin/bash

# A script (scan-logs.sh) to search /var/log/auth.log for "Failed password" attempts and save results to failed-logins.txt.


log_file="/var/log/auth.log"
failed_login="/home/mohsin/failed-logins.txt"
malicious_ip="/home/mohsin/malicious-ip"

# Extracting the failed password logs and stored the ouput in file

grep -i "failed password" "$log_file" >> "$failed_login"


# Extract the ips and identifying the uniques ones

awk '{print $(NF-3)}' failed-login.txt | sort | uniq > "$malicious_ip"

# Blocking all the IP stored in the malicious_ip files

# using while loop to block all the ips one by one.

while read -r mal_IP; do

        # Blocking IP with ufw 
        sudo ufw deny from "$mal_IP"
        echo "$mal_IP is blocked"

done<"malicious_ip" # Giving the input file.

# restart the ufw service

systemctl restart ufw

```

Schedule the script to run hourly.

```
crontab -e
```

add the entry

```
0 * * * * /home/mohsin/scan-logs.sh
```


User Management & Security Hardening  
Objective: Secure user accounts and audit system permissions.  
Tasks:

Create two users: dev1 and dev2 with home directories.

Force both users to reset passwords on first login.

Create a group developers and add both users to it.

Restrict SSH access to members of the developers group.

Find all files with SUID/SGID permissions and document them in suid-report.txt.




