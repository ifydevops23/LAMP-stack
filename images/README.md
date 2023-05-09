# LAMP Stack Installation

## Background
A technology stack is a set of frameworks and tools used to develop a software product. 
This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronyms for individual technologies used together for a specific technology product. some examples are…
- LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
- LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
- MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
- MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

## STEP 0 : PREREQUISITE
- Launch a virtual server with Ubuntu Server OS.
- Connect to the remote host via SSH.

## STEP 1 : INSTALLING APACHE AND UPDATING THE FIREWALL (Web Server)
- #Update a list of packages in package manager
`sudo apt update` 
- #Run apache2 package installation
`sudo apt install apache2`
- #Check Apache status
`sudo systemctl status apache2`
- #Verify that apache2 is running as a Service in our OS
`sudo systemctl status apache2`
- #Access url from a web browser 
`http://<Public-IP-Address>:80` or `curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

## STEP 2 — INSTALLING MYSQL (Database Management System)
- #To acquire and install this software
`sudo apt install mysql-server`
- #Log in to the MySQL console
`sudo mysql`
- #Set a password for the root user
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'; `
- #Exit the MySQL shell with
`exit` after the mysql> prompt
- #Start the interactive script by running
`sudo mysql_secure_installation`
- #Test if you’re able to log in to the MySQL console
`sudo mysql -p`
- #Exit the MySQL console
`exit`

## STEP 3 — INSTALLING PHP (Required Libraries and Modules for Dynamic Content)
- #To install PHP package, php-mysql, libapache2-mod-php
`sudo apt install php libapache2-mod-php php-mysql`
- #To confirm php version
`php -v`

## STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
Virtual host allows you to have multiple websites located on a single machine.
Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. 
- #Create the directory different from the default directory
`sudo mkdir /var/www/<domain_name>`
- #Assign ownership of the directory with your current system user:
` sudo chown -R $USER:$USER /var/www/<domain_name>`
- #create and open a new configuration file in Apache’s sites-available directory
`sudo vi /etc/apache2/sites-available/<domain_name>.conf`
This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
```<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- #Use the ls command to show the new file in the sites-available directory
`sudo ls /etc/apache2/sites-available`
- #Use a2ensite command to enable the new virtual host
`sudo a2ensite projectlamp`
