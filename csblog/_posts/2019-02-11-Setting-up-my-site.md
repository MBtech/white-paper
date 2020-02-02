---
layout: post
title: "Setting up my site"
categories:
  - tutorial
  - csblog
tags:
  - tutorial
---
# Setting up my site
Took a long while to get to it but I finally managed to setup a blog site on a server I manage myself.
In this post, I will explain how I got this site up and running.

### Step 1: Buy a domain
Get a domain name! I used namecheap since I had already bought a domain name from there. You can use any of the whole
wide range of domain name providers out there. If you just want to experiment, there are domain names that are less than a dollar/euro.

### Step 2: Get a Server
You don't always have to spend money to get a server enough to serve a small site. For example you can use the GCP free tier. They have an always 1 f1-micro VM instance per month with certain limitations. You can get all the information about usage limits [here](https://cloud.google.com/free/docs/gcp-free-tier).
But that instance is only suitable for low-traffic website. Or you can just get a virtual server from any number of cloud providers.

Regardless of the cloud provider, make sure that the firewall rules of your instance allow for atleast http traffic. For GCP you can modify the firewall rules from [here](https://cloud.google.com/vpc/docs/using-firewalls).
### Step 3: Setup DNS entry for your Server


### Step 4: Install Apache webserver
Use [this guide](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04) to install apache webserver, if it's not already there. The guide is for Ubuntu.
Or simply use the following commands
```
sudo apt-get update
sudo apt-get install apache2
sudo service apache2 start
```

### Step 5: Configure Apache webserver
By default apache webserver serves the files in your /var/www/html directory. We will do the setup such that our built site is copied to a directory in /var/www/ and that files from that directory is served by the apache webserver.

```
sudo vi /etc/apache2/sites-available/default
```
Modify the file so that it looks something like this.
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost

        DocumentRoot /var/www/yoursite/
        <Directory />
                Options FollowSymLinks
                AllowOverride None
        </Directory>
        <Directory /var/www/yoursite/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>
```
You can read further about apache webserver configuration [here](https://www.digitalocean.com/community/tutorials/how-to-configure-the-apache-web-server-on-an-ubuntu-or-debian-vps). 
### Step 6: Setup for Jekyll
```
sudo apt install rubygems
gem install jekyll
```

### Step 7: Setup auto-regeneration
I used pygithub and modified the example of the webhook that they have [here](https://pygithub.readthedocs.io/en/latest/examples/Webhook.html#creating-and-listening-to-webhooks-with-pygithub-and-pyramid)



**References**
