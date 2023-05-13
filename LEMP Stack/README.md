# WEB STACK IMPLEMENTATION (LEMP STACK)

In this project, I implemented a similar stack to LAMP, but with an alternative Web Server – NGINX, which is also very popular and widely used by many websites on the Internet.
The acronym LEMP represents Linux, eNginx, MySQL, PHP

## STEP 0 - Requirements
- An Ubuntu Virtual Machine

![0_PreReq_Ubuntu_VM](https://github.com/ifydevops23/Software_Stack/assets/126971054/9391c565-7df0-4cff-a6a2-5813f38ac739)

- SSH client for connecting to the VM

## STEP 1 - INSTALLING THE NGINX WEB SERVER
- Update the server package index by typing `sudo apt update`.
- Use `apt install` to get Nginx installed.

![1_install_nginx](https://github.com/ifydevops23/Software_Stack/assets/126971054/2420e30a-b2cc-4272-b6a9-dfcb9c49d8f2)

- Verify that nginx was successfully installed and is running as a service in Ubuntu `sudo systemctl status nginx`.

![1_nginx_status](https://github.com/ifydevops23/Software_Stack/assets/126971054/7ab9c751-e069-4164-a28a-dadcce579951)

- To access it locally in our Ubuntu shell, run `curl http://localhost:80` or curl http://127.0.0.1:80 via a web browser.

![1_success_from_curl1a](https://github.com/ifydevops23/Software_Stack/assets/126971054/fcdbb8e5-15d5-4e7c-9880-189b19606f6f)

## STEP 2 — INSTALLING MYSQL  (Database Management System (DBMS) to store and manage data for your site)

- Use `sudo apt install mysql-server` to acquire and install this software.

![2_install_mysql_server](https://github.com/ifydevops23/Software_Stack/assets/126971054/8ad37319-bdba-4806-a806-77acd827df90)

- When prompted, confirm installation by typing Y, and then ENTER.
- When done, log in to the MySQL console by typing `sudo mysql`

![2_mysql_login](https://github.com/ifydevops23/Software_Stack/assets/126971054/61172ae9-09ac-4f3b-aca3-ddcc82069789)

- Remove some insecure default settings and lock down access to your database system by typing `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`. 

![2_mysql_default_password](https://github.com/ifydevops23/Software_Stack/assets/126971054/7a5124cb-0a36-4ea8-9e32-9ba1cd84f34e)

- Set a password for the root user, using mysql_native_password as default authentication method `sudo mysql_secure_installation`.

![2_secure_server_root_passwd_change](https://github.com/ifydevops23/Software_Stack/assets/126971054/d50e0de3-017f-43e0-bbe6-ed1a36912271)

- When you’re finished, test if you’re able to log in to the MySQL console by typing `sudo mysql -p`
- To exit the MySQL console, type `exit`

## STEP 3 – INSTALLING PHP ( To process code and generate dynamic content for the web server)

I installed php-fpm, which stands for “PHP fastCGI process manager”, which tells Nginx to pass PHP requests to this software for processing and php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases.

- To install these 2 packages at once `sudo apt install php-fpm php-mysql`

![3_insatll_php_modules_and_lib](https://github.com/ifydevops23/Software_Stack/assets/126971054/c2542375-c106-4409-9af8-ab2b7a55e2d2)

- When prompted, type Y and press ENTER to confirm installation.

## STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use project LEMP as an example domain name.

- Create the root web directory for your domain as follows `sudo mkdir /var/www/projectLEMP 
- Assign ownership of the directory with the $USER environment variable `sudo chown -R $USER:$USER /var/www/projectLEMP`

![4_Configuring_nginx_to_use_php](https://github.com/ifydevops23/Software_Stack/assets/126971054/ad5cdb2b-ed3e-462a-bff6-348e431fdf2a)

- Open a new configuration file in Nginx’s sites-available directory `sudo nano /etc/nginx/sites-available/projectLEMP` 
```
#/etc/nginx/sites-available/projectLEMP 
server {
	listen 80;
	server_name projectLEMP www.projectLEMP;
	root /var/www/projectLEMP;
 
	index index.html index.htm index.php;
 
	location / {
    	try_files $uri $uri/ =404;
	}
 
	location ~ \.php$ {
    	include snippets/fastcgi-php.conf;
    	fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
 	}
 
	location ~ /\.ht {
    	deny all;
	}
 
}
```

- Activate your configuration by linking to the config file from Nginx’s sites-enabled directory `sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`
- You can test your configuration for syntax errors by typing `sudo nginx -t`

![4_checking_config_file_syntax](https://github.com/ifydevops23/Software_Stack/assets/126971054/4f3892cc-f6fa-451d-a04f-43b6cc3a0c77)

- Disable default Nginx host that is currently configured to listen on port 80, for this run: `sudo unlink /etc/nginx/sites-enabled/default`
- Reload Nginx to apply the changes: `sudo systemctl reload nginx`
- Create an index.html file under projectLEMP directory
`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`
- Go to your browser and try to open your website URL using IP address [http://<Public-IP-Address>:80]

![4_success_from web_browser](https://github.com/ifydevops23/Software_Stack/assets/126971054/e7c470f1-a83d-4ffb-85a5-034785075922)

## STEP 5 – TESTING PHP WITH NGINX
Test it to validate that Nginx can correctly hand .php files off to your PHP processor.
Create a test PHP file in your document root.
- Open a new file called info.php within your document root in your text editor `sudo nano /var/www/projectLEMP/info.php`
- Type or paste the following lines into the new file.
```
<?php
phpinfo();
```
-  Access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php `http://`server_domain_or_IP`/info.php`

![5_php_from_browser](https://github.com/ifydevops23/Software_Stack/assets/126971054/52397c26-d6b0-4aad-8254-8789c029c5d2)

- Remove the info.php file `sudo rm /var/www/your_domain/info.php`

## STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
In this step I created a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

- Connect to the MySQL console using the root account `sudo mysql -p`
- Create a new database `mysql> CREATE DATABASE `example_database`;`
- Create a new user named example_user `mysql> CREATE USER `'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

![My_sql_prompt](https://github.com/ifydevops23/Software_Stack/assets/126971054/17290cdc-3cb2-4095-abf3-83ca448a33cd)

- Give this user permission over the example_database database `mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`
- Now exit the MySQL shell with `mysql> exit`
- Test if the new user has the proper permissions by logging in to the MySQL console `mysql -u example_user -p`
- Confirm that you have access to the example_database database `mysql> SHOW DATABASES;`

![6_Show_database_(default_DB)](https://github.com/ifydevops23/Software_Stack/assets/126971054/933c1b82-a88c-48c5-a456-7ec3d3b01dab)

- Create a test table named todo_list
```
CREATE TABLE example_database.todo_list (
mysql> 	item_id INT AUTO_INCREMENT,
mysql> 	content VARCHAR(255),
mysql> 	PRIMARY KEY(item_id)
mysql> );

```

- Insert a few rows of content in the test table `mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

![6_mysql_table_creation_plus_content](https://github.com/ifydevops23/Software_Stack/assets/126971054/6b578416-555b-4a79-8ae3-6dc079493b0d)

- To confirm that the data was successfully saved to your table, run: `mysql> SELECT * FROM example_database.todo_list;`

![6_DB_table_from_terminal](https://github.com/ifydevops23/Software_Stack/assets/126971054/fa7b2f82-74d2-491b-977f-4711ce2bea20)

- After confirming that you have valid data in your test table, you can exit the MySQL console `mysql> exit`
- Create a PHP script that will connect to MySQL and query for your content `nano /var/www/projectLEMP/todo_list.php` and copy this content into your todo_list.php script:

```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";
 
try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
	echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
	print "Error!: " . $e->getMessage() . "<br/>";
	die();
}

```
- Save and close the file when you are done editing.
- You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php [http://<Public_domain_or_IP>/todo_list.php]

![Hello_from_db_table](https://github.com/ifydevops23/Software_Stack/assets/126971054/ec3ce2af-8b16-4836-996c-f674f1aa61f9)



