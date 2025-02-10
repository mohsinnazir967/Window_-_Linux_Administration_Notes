
# Task 1: Web Server Setup & Automation  

## Objective:

Set up a Nginx web server with automated deployment of a static website.


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
sudo ln -s /etc/nginx/sites-avaiable/lab-site /etc/sites-enabled/lab-site
```



**Write a script (deploy.sh) to automate copying files from /home/user/web-files to /var/www/lab-site and reload Nginx.**



**Use cron to run the script every day at 2 AM.**