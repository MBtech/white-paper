---
layout: post
title: "Installing Software Packages behind Firewall"
categories:
  - tutorial
  - csblog
tags:
  - content
---

## Installing Software Packages behind Firewall
Recently, I had a scenario in which I had to install software packages on nodes that are behind firewalls with no access to internet. Only one node on the LAN is connected to internet while the rest of the nodes aren't.

To get the nodes configured there are two options:
- Either download the deb package manually and install them
- Use a proxy to allow connecting the local nodes through the head node to internet.

Following figure shows the network configuration that I am dealing with. We have a LAN on subnet 192.168.4.0/24. The headnode has IP 192.168.4.100. The nodes that we want to setup software packages on have IPs in ranges 192.168.4.101-192.168.4.120.

### Setup Proxy Server
To do this we will setup a proxy server at the head node. We will use Squid proxy server. Install the proxy server using:

`sudo apt install squid3`

Create a copy of original squid conf so that if we mess things up we have the original copy
`sudo cp /etc/squid3/squid.conf /etc/squid3/squid.conf.original`
To set your Squid server to listen on TCP port 8888 instead of the default TCP port 3128, change the http_port directive as such: `http_port 8888`. For our example we will simply use the

Using Squid's access control, you may configure use of Internet services proxied by Squid to be available only users with certain Internet Protocol (IP) addresses. For example, we will illustrate access by users of the 192.168.4.0/24 subnetwork only:

Add the following to the bottom of the ACL section of your `/etc/squid3/squid.conf` file:

`acl fortytwo_network src 192.168.4.0/24`
Then, add the following to the top of the http_access section of your /etc/squid3/squid.conf file:

`http_access allow fortytwo_network`

After making changes to the /etc/squid3/squid.conf file, save the file and restart the squid server application to effect the changes using the following command entered at a terminal prompt:

`sudo systemctl restart squid3.service`

#### Using proxy with apt
Edit configuration file for apt using

`sudo /etc/apt/apt.conf`

Paste this line in the apt.conf
`Acquire::http::proxy "http://192.168.4.100:3128/";`

You can also specify proxy option directly while using apt
`sudo http_proxy='http://192.168.4.100:3128/' apt-get install package-name`


#### Configure proxy for python-pip
`sudo pip install Flask-MongoAlchemy --proxy="http://192.168.4.100:3128"`

#### Configure Maven with proxy
```
<settings>
  .
  .
  <proxies>
   <proxy>
      <id>example-proxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>proxy.example.com</host>
      <port>8080</port>
      <username>proxyuser</username>
      <password>somepassword</password>
      <nonProxyHosts>www.google.com|*.example.com</nonProxyHosts>
    </proxy>
  </proxies>
  .
  .
</settings>
```
Adding this to your pom.xml with respective changes will allow Maven to fetch the dependencies using
the specified proxy.

**References**
- https://help.ubuntu.com/lts/serverguide/squid.html
- http://stackoverflow.com/questions/19080352/how-to-get-pip-to-work-behind-a-proxy-server
- https://maven.apache.org/guides/mini/guide-proxies.html
