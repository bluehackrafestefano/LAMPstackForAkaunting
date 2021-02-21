# Task-1:
## LAMPstackForAkauntingTask
This repo is created to execute Akaunting task by creating a LAMP stack with Github Actions.
### Step 1: Preparing Ubuntu server
- Create an Ubuntu server on cloud and connect to it using SSH
- Initially it is an "Ubuntu Server 20.04 LTS (HVM), SSD Volume Type - ami-03d315ad33b9d49c4", t2.micro, on us-east-1 region, allows SSH connection on port 22, and also port 80 and 443 are open for http/s connections on AWS. 
- Update the server:
```sh
sudo apt update
sudo apt upgrade -y
```
- Open ports 22 (for SSH), 80 and 443 and enable Ubuntu Firewall (ufw):
```sh
sudo ufw allow ssh
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```
### Step 2: Installing and testing Apache2
- Install Apache using apt:
```sh
sudo apt install apache2 -y
```
```sh
# Starting Apache server:
# sudo service apache2 start
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
### Step 3: Installing and testing PHP 7.4
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
```sh
# Restarting Apache server:
# sudo service apache2 restart
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
### Step 4: Installing and securing MariaDB
- Install the required packages:
```sh
sudo apt install mariadb-server mariadb-client -y
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
- Turn on password authentication, which is not by default:
```sh
sudo mysql
```
```sql
select user, authentication_string from myswl.user;

alter user root@localhost identified with mysql_native_password by 'password';

flush privilages;

exit
```
- Test this:
```sh
mysql -u root -p
```
- And type root password you specified!
### Step 5: Creating Basic PHP-enabled page
- Create a file named hello.php and put it under /var/www/html/ with the following content:
```bash
cd /var/www/html/ && sudo nano hello.php 
```
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
### Step 6: Create a self hosted Github Action
- Open the CLI, on your ubuntu server, first create a user, this is necessary to implement self hosting:
```sh
adduser username
```
- Configure password and other credientials
- Add that user to sudo group:
```sh
adduser username sudo
```
- Login as the new user:
```sh
sudo su - newuser
```
- Go to Github account and create a private repository, give a name
- Go to Settings of your repo
- Click Actions on the left side menu
- Scroll down and click Add runner on Self-hosted runners window
- Change OS to Linux
- Implement the steps shown on the page, open CLI again:
- Create a folder:
```sh
mkdir actions-runner && cd actions-runner
```
- Download the latest runner package:
```sh
curl -O -L https://github.com/actions/runner/releases/download/v2.277.1/actions-runner-linux-x64-2.277.1.tar.gz
```
- Extract the installer:
```sh
tar xzf ./actions-runner-linux-x64-2.277.1.tar.gz
```
- Create the runner and start the configuration experience:
```sh
./config.sh --url https://github.com/\<reponame> --token \<token>
```
- Enter name of the runner if you want, else hit Enter
- Enter additional labels if you want, else hit Enter
- Enter work folder if you want, else hit Enter
- Last step, run it:
```sh
./run.sh
```
- Server starts to listen for Jobs from Github Actions
- Create a simple workflow file on the repo ".github/workflows/blank.yml"
```yml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the action will run. 
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: self-hosted

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```
- Now, our github actions running on self-hosted, Digital Ocean server.  
### Step 7: Basic PHP Unit Test
- The easiest way to obtain PHPUnit is to download a PHP Archive (PHAR) that has all required (as well as some optional) dependencies of PHPUnit bundled in a single file.
- The phar extension is required for using PHP Archives (PHAR).
- The PHPUnit PHAR can be used immediately after download:
- Create a folder phpunittest
- 


# Task-2:
- Akaunting, as far as I know, has one development team, working on software projects.
- 
