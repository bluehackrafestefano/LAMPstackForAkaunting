# Task-1:
## LAMPstackForAkauntingTask
This repo is created to execute Akaunting task by creating a LAMP stack with Github Actions.
## Step 1: Preparing Ubuntu server
- Create an Ubuntu server on cloud and connect to it using SSH
- Initially it is an "Ubuntu Server 20.04 LTS (HVM), SSD Volume Type - ami-03d315ad33b9d49c4", t2.micro, on us-east-1 region, allows SSH connection on port 22, and also port 80 and 443 are open for http/s connections on AWS. 
- Update the server:
```sh
sudo apt update
sudo apt upgrade -y
```
<!-- -Now open ports 22 (for SSH), 80 and 443 and enable Ubuntu Firewall (ufw):
```sh
sudo ufw allow ssh
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
``` -->
## Step 2: Installing and testing Apache2
- Install Apache using apt:
```sh
sudo apt install apache2 -y
```
- Confirm that Apache is now running with the following command:
```sh
sudo systemctl status apache2
```
- You should get an output showing that the apache2.service is running and enabled.
- Once installed, test by accessing your server’s IP in your browser:
```sh
# curl -4 icanhazip.com  ## to get the IP
http://YOURSERVERIPADDRESS/
```
- You should see a page with an “Apache2 Ubuntu Default” header showing that Apache2 has been installed successfully.
## Step 3: Installing and testing PHP 7.4
- Let’s install PHP 7.4 as a stable version:
```sh
sudo apt install php7.4 php7.4-mysql php-common php7.4-cli php7.4-json php7.4-common php7.4-opcache libapache2-mod-php7.4 -y
```
- Check the installation and version:
```sh
php --version
```
- Restart Apache for the changes to take effect:
```sh
sudo systemctl restart apache2
```
- Create a phpinfo.php test page:
```sh
echo '<?php phpinfo(); ?>' | sudo tee -a /var/www/html/phpinfo.php > /dev/null
```
- Test everything is working by accessing the following in your browser:
```sh
# curl -4 icanhazip.com  ## to get the IP
http://YOURSERVERIPADDRESS/phpinfo.php
```
- You should see a PHP Version 7.4.3 page listing all of your PHP options.
- Once you’ve confirmed that PHP is working correctly, delete the test file.
```sh
sudo rm /var/www/html/phpinfo.php
```
## Step 4: Installing and securing MariaDB
- Install the required packages:
```sh
sudo apt install mariadb-server mariadb-client
```
- Once installed, check it’s running correctly:
```sh
sudo systemctl status mariadb
```
- Secure your newly installed MariaDB service:
```sh
sudo mysql_secure_installation
```
- As you have no root password set for MariaDB you should simply press Enter when prompted, pressing Y on the next question to then set a root password (keep this safe and secure!) With that set, you can press Enter for the remaining questions as the defaults for each of these will help to secure your new installation.
## Basic PHP-enabled page
- Create a file named hello.php and put it under /var/www/html/ with the following content:
```html
<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <?php echo '<p>Hello World</p>'; ?> 
 </body>
</html>
```
- Test "hello world" php page is working by accessing the following in your browser:
```sh
# curl -4 icanhazip.com  ## to get the IP
http://YOURSERVERIPADDRESS/hello.php
```
# Task-2:
- Akaunting, as far as I know, has one development team, working on software projects.
- 
