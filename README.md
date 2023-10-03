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

## Step 2:

![ec2 1](<images/server 1.png>)

![ec2 2](<images/server 2.png>)


> Open port 8000 to allow traffic from everywhere.

> Add a rule to the security group of each of the webservers.

![add rule 1](<images/add rule 1.png>)

![add rule 2](<images/add rule 2.png>)




