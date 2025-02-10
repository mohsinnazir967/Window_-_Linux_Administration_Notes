
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
```