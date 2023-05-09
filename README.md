# LAMP Stack Installation

## Background
A technology stack is a set of frameworks and tools used to develop a software product. 
This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. There are acronyms for individual technologies used together for a specific technology product. some examples are…
- LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
- LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
- MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
- MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

## STEP 0 : PREREQUISITE
- Launch a virtual server with Ubuntu Server OS.
- Connect to the remote host via SSH.

![Virtual Machine- Running](https://github.com/ifydevops23/LAMP-stack/assets/126971054/81a52e3e-ac7d-42cf-a7c2-5d9fd13ab282)

## STEP 1 : INSTALLING APACHE AND UPDATING THE FIREWALL (Web Server)
- Update a list of packages in package manager
`sudo apt update` 
- Run apache2 package installation
`sudo apt install apache2`
- Check Apache status
`sudo systemctl status apache2`
![Apache_running](https://github.com/ifydevops23/LAMP-stack/assets/126971054/3af10f6a-7831-487c-bcdc-c5bd8f05b394)

- Verify that apache2 is running as a Service in our OS
`sudo systemctl status apache2`
- Access url from a web browser 
`http://<Public-IP-Address>:80` or `curl -s http://169.254.169.254/latest/meta-data/public-ipv4`
![Apache_interface_via_web](https://github.com/ifydevops23/LAMP-stack/assets/126971054/f0feeca1-0770-41db-9c1f-6a77d7b8a732)

## STEP 2 — INSTALLING MYSQL (Database Management System)
- To acquire and install this software
`sudo apt install mysql-server`
![Installin_mysql_server](https://github.com/ifydevops23/LAMP-stack/assets/126971054/ee927f23-e01c-4453-b4a7-d96a470a98d2)
- Log in to the MySQL console
`sudo mysql`
- Set a password for the root user
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'; `
![Password_change](https://github.com/ifydevops23/LAMP-stack/assets/126971054/7721bbaa-64fc-4e1c-8aa2-a5894e0f78a1)
- Exit the MySQL shell with
`exit` after the mysql> prompt
- Start the interactive script by running
`sudo mysql_secure_installation`
- Test if you’re able to log in to the MySQL console
`sudo mysql -p`
- Exit the MySQL console
`exit`

## STEP 3 — INSTALLING PHP (Required Libraries and Modules for Dynamic Content)
- To install PHP package, php-mysql, libapache2-mod-php
`sudo apt install php libapache2-mod-php php-mysql`
![installing-php](https://github.com/ifydevops23/LAMP-stack/assets/126971054/7f1fb09a-ee78-4e84-839b-a1b7ddbe671e)
- To confirm php version
`php -v`

## STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
Virtual host allows you to have multiple websites located on a single machine.
Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. 
- Create the directory different from the default directory
`sudo mkdir /var/www/<domain_name>`
- Assign ownership of the directory with your current system user:
` sudo chown -R $USER:$USER /var/www/<domain_name>`
- Create and open a new configuration file in Apache’s sites-available directory
`sudo vi /etc/apache2/sites-available/<domain_name>.conf`.
This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
```
<VirtualHost *:80>
    ServerName <domain_name>
    ServerAlias www.<domain_name> 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/<domain_name>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Use the ls command to show the new file in the sites-available directory
`sudo ls /etc/apache2/sites-available`
- Use a2ensite command to enable the new virtual host
`sudo a2ensite projectlamp`
- To disable Apache’s default website use:
`sudo a2dissite 000-default`
- To make sure your configuration file doesn’t contain syntax errors, run:
`sudo apache2ctl configtest`
- Reload Apache so these changes take effect:
`sudo systemctl reload apache2`
-  Create an index.html file in that location so that we can test that the virtual host works 
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/<domain_name>/index.html
```
- Open your website URL using IP address
`http://<Public-IP-Address>:80`
![hello_from_new_website](https://github.com/ifydevops23/LAMP-stack/assets/126971054/d5fadd55-8214-4a6c-b8cd-246499362763)

## STEP 5 — ENABLE PHP ON THE WEBSITE
### Setting up index.php as landing page to take precedence over index.html
- To edit the /etc/apache2/mods-enabled/dir.conf file
`sudo vim /etc/apache2/mods-enabled/dir.conf` then type in,
```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
- Reload Apache so the changes take effect:
`sudo systemctl reload apache2`
### Create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
- Create a new file named index.php inside your custom web root folder:
`vim /var/www/<domain_name>/index.php`
- Type:
```
<?php
phpinfo();
```
- Refresh the page at `http://<Public-IP-Address>:80`

![php_enabled](https://github.com/ifydevops23/LAMP-stack/assets/126971054/7a67034a-75ce-4947-ac83-0665dae51e91)

- Remove index.php file created (contains sensitive information about the PHP environment and server)
`sudo rm /var/www/<domain_name>/index.php`
