---
layout: post
title: "Setting up a Hotcrp server"
categories:
  - tutorial
tags:
  - tutorial, hotcrp, ubuntu, installation guide
---
## Installing Hotcrp on your server

For a research paper reading course, I was asked to setup a hotcrp installation so that students could submit their reviews of the paper through the method that is used by actual reviewers. So here is how I install hotcrp on a Bodhi linux based on Ubuntu 16.04. However, the procedure should be the same on mainstream Ubuntu 16.04 as well.

1. Install required packages:
``` shell
sudo apt update
sudo apt install git apache2 php-common php-gd php-mysql libapache2-mod-php zip postfix mailutils libsasl2-2 ca-certificates libsasl2-modules mysql-server python-poopler
git clone https://github.com/kohler/hotcrp.git
```
**Note:** use a strong password for the MySQL root user when asked during the installation of MySQL server and do not leave it blank.

2. When the installation is complete, we want to run a simple security script that will remove some dangerous defaults and lock down access to our database system a little bit. Start the interactive script by running:
```
sudo mysql_secure_installation
```
You will be asked to enter the password you set for the MySQL root account. Next, you will be asked if you want to configure the VALIDATE PASSWORD PLUGIN. Details of whether you want to configure it or not can be found [here](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04). For the rest of the prompts you should use **Y**.

3. Create database for hotcrp
```
lib/createdb.sh --user=root --password=root_password_here
```
You can also use another MySQL user and the respective password. You will be asked to enter a database name that you want to use for hotcrp. This information will be stored in `conf/options.php`. Edit the file to change other options such as `contactName`, `contactEmail`, `sendEmail`, `emailFrom` etc.

   ```
   vi conf/options.php
   ```

4. Add a directory to point `/testconf` path  to point to hotcrp installation by editing the configuration file `/etc/apache2/apache2.conf`. Replace the path to hotcrp below with the path that you have for your installation.

  ```
  <Directory "/home/user_name/hotcrp">
       Options Indexes Includes FollowSymLinks
       AllowOverride all
       Require all granted
   </Directory>
   Alias /testconf /home/user_name/hotcrp
   ```
5. Update the following PHP settings in `.htaccess` and `.user.ini`
 -  `upload_max_filesize`: Set to the largest file upload HotCRP should accept. 15M is a good default.

 - `post_max_size`: Set to the largest total upload HotCRP should accept. Must be at least as big as `upload_max_filesize`. 20M is a good default.

 - `max_input_vars`: Set to the largest number of distinct input variables HotCRP should accept. 4096 is a good default.

 - The last setting, `session.gc_maxlifetime`, must be changed globally. This provides an upper bound on HotCRP session lifetimes (the amount of idle time before a user is logged out automatically). On Unix machines, systemwide PHP settings are often stored in `/etc/php/7.0/apache2/php.ini`. The suggested value for this setting is 86400, e.g., 24 hours:

    `session.gc_maxlifetime = 86400`

    If you want sessions to expire sooner, we recommend you set `session.gc_maxlifetime` to 86400 anyway, then edit `conf/options.php` to set `$Opt["sessionLifetime"]` to the correct session timeout.
6. Edit MySQL conf `/etc/mysql/my.cnf` to ensure that it can handle the correct sized objects. Add the following to the configuration File

  ```
  [mysqld]
  max_allowed_packet=32M
  ```
Restart the mysql server `sudo /etc/init.d/mysql restart`
7. Now we will setup postfix for relaying messages through smtp servers. In this example I will use the gmail smtp servers.

  Edit `/etc/postfix/main.cf` and add the following lines. If any of them are present modify them

  ```
  relayhost = [smtp.gmail.com]:587
  smtp_sasl_auth_enable = yes
  smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
  smtp_sasl_security_options = noanonymous
  smtp_tls_CAfile = /etc/postfix/cacert.pem
  smtp_use_tls = yes
  smtp_tls_policy_maps = hash:/etc/postfix/tls_policy
  ```
  Open/create `/etc/postfix/sasl_passwd` and add the following line to it replacing username and password with the credentials to the gmail account that you want to use

  ```
  [smtp.gmail.com]:587    USERNAME@gmail.com:PASSWORD
  ```
  Fix the permissions and update postfix conf to use the sasl_passwd file

  ```
  sudo chmod 400 /etc/postfix/sasl_passwd
  sudo postmap /etc/postfix/sasl_passwd
  ```

  Validate certificates to avoid any errors using

  ```
  cat /etc/ssl/certs/thawte_Primary_Root_CA.pem | sudo tee -a /etc/postfix/cacert.pem
  ```
  *You might need to use `Thawte_Premium_Server_CA.pem` if the other cert is not present on your system.*

  Edit `/etc/postfix/tls_policy` and add the following line to the file

  ```
  [smtp.gmail.com]:587 encrypt
  ```
  then run

  ```
  sudo postmap /etc/postfix/tls_policy
  ```

  Finally, reload postfix config and restart postfix service

  ```
  sudo /etc/init.d/postfix reload
  systemctl restart postfix.service
  ```
8. First authenticate from your server using [this page](https://www.google.com/accounts/DisplayUnlockCaptcha). Now you can test your postfix setup using the following command and replace the destination email with the email address to which you want to send this test email.

  ```
  echo "Test mail from postfix" | mail -s "Test Postfix" destination_email@gmail.com
  ```
  For debugging and error info use the command `journalctl` to look into the logs.
9. Now restart the apache server

  ```
  sudo apache2ctl graceful
  ```
  and try to access the hotcrp webpage via `http://localhost/testconf`.

  For further details about the setup and more on hotcrp you can look at the README in hotcrp repository https://github.com/kohler/hotcrp


**References**
- https://www.howtoforge.com/tutorial/configure-postfix-to-use-gmail-as-a-mail-relay/
- https://easyengine.io/tutorials/linux/ubuntu-postfix-gmail-smtp/
- https://github.com/kohler/hotcrp
- https://support.google.com/accounts/answer/6010255
