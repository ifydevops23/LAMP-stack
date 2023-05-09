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
- 

