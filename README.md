# Load-Balancing-and-Nginx - ***Armstrong***
Introduction to Load Balancing and Nginx

![laod balancing](<images/load balancing.png>)

Load balacing is team work for compters. 

Loading balancing means distributing the work or tasks across multiple computers or servers sos that no one computer gets overloaded with too much work.

Nginx is a versertile software that can be configured to act like a webserver; reverse proxy and a load balancer.


---
# Setting up a basic Load Balancer.

## Step 1:

> Provision two EC2 instances

![ec2 instances](<images/ec2 instances.png>)

> two ec2 instances are running now

![ec2 1](<images/server 1.png>)

![ec2 2](<images/server 2.png>)


## Step 2:


> Open port 8000 to allow traffic from everywhere.

> Add a rule to the security group of each of the webservers.

![add rule 1](<images/add rule 1.png>)

![add rule 2](<images/add rule 2.png>)

## Step 3:

> Connect to instances

![connect to instances](<images/connect to instances.png>)

> Install Apache Webserver

```
sudo apt update -y &&  sudo apt install apache2 -y
```
![install appache](<images/install apache.png>)

> Verify Apache is running

```
sudo systemctl status apache2
```
![apache running](<images/apache running.png>)

## Step 4:

> Configure Apache to serve a page showing it public ip

> Configure Apache to serve content on port 8000

> Open /etc/apache2/ports.conf , and add a new directive for port 8000

```
sudo vi /etc/apache2/ports.conf 
```
![listen 8000](<images/listen 8000.png>)

> Open the file /etc/apache2/sites-available/000-default.conf , and change port 80 on the virtualhost to 8000.

```
sudo vi /etc/apache2/sites-available/000-default.conf
```
![change port to 8000](<images/change to 8000.png>)

> Restart Apache to load the new configuration

```
sudo systemctl restart apache2
```

> Create new index.html file and paste code

```
sudo vi index.html
```
Instance 1
```
        <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: 18.130.214.86</p>
        </body>
        </html>
```
Instance 2
```
        <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: 52.56.226.226</p>
        </body>
        </html>
```
![index.html file](<images/index.html file.png>)

> Change File ownership of index.html file

```
sudo chown www-data:www-data ./index.html
```

> Overide the default html file of the Apache webserver

```
sudo cp -f ./index.html /var/www/html/index.html
```

> Restart webserver to load new configurations

```
sudo systemctl restart apache2
```
![restart apache](<images/restart apache.png>)

![web 1](<images/web 1.png>)

![web 2](<images/web 2.png>)

## Step 5:

> Configuring Nginx as a Load Balancer

> Provision a new EC2 instance

![3rd instance](<images/3rd instance.png>)

>SSH into instance

> Install Nginx

```
sudo apt update -y && sudo apt install nginx -y
```

> Verify Nginx is installed and working

```
sudo systemctl status nginx
```
![install nginx](<images/instal nginx.png>)


> Open Nginx configuration file

```
sudo vi /etc/nginx/conf.d/loadbalancer.conf
```
> Paste configuration into file

```
        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server 127.0.0.1:8000; # public IP and port for webserser 1
            server 127.0.0.1:8000; # public IP and port for webserver 2

        }

        server {
            listen 80;
            server_name <your load balancer's public IP addres>; # provide your load balancers public IP address

            location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
```

![nginx file configure](<images/nginx file configure.png>)

> Test configuration

```
sudo nginx -t
```
![nginx successful](<images/nginx successful.png>)

> Restart Nginx

```
sudo systemctl restart nginx
```

> Paste public ip of Nginx Load balancer in web.


