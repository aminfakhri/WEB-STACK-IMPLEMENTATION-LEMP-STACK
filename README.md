# WEB-STACK-IMPLEMENTATION-LEMP-STACK
In this project I will implement Linux OS, NGINX, MySQL, and PHP.
Using AWS account and deploy ubuntu EC2 instance 22.04 HVM image.

the image below shows connecting to the EC2 instance using VScode after deployed the instance 
![after deploying instance and ssh using VScode](https://user-images.githubusercontent.com/96633325/225006180-c0605d5b-db3d-403e-8236-440af34c741a.PNG)




# STEP 1 - installing Nginx web server

I deployed new server in AWS so its needs update and get packeges 

$sudo apt update
$sudo apt install nginx

after completing the installation and need to verify the installation was successfull type

$sudo systemctl status nginx

if it shows running in green font this means its installed and running

![verify ngnix is running](https://user-images.githubusercontent.com/96633325/225006286-722bcb6d-1a0a-4932-9a0b-bdc9580ec931.PNG)

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is default port that web brousers use to 
access web pages in the Internet.

![adding indound rule in security group HTTP80](https://user-images.githubusercontent.com/96633325/225006865-44b09d32-3357-4371-9dde-d3b218909e72.PNG)

now we need to check how we can access it locally in our Ubuntu shell
curl http://localhost:80


![access web service locally](https://user-images.githubusercontent.com/96633325/225007608-230b0337-5625-4687-9ae9-4824c7bf8a82.PNG)

Accessing the web service through web browser using public ip of the EC2 instance

![access the web service through web browser](https://user-images.githubusercontent.com/96633325/225008198-67700e11-e0ad-402c-917d-502383e4ba08.PNG)

# STEP 2 Installing MySQL

Now we have deployed web server and running, we need to install DBMS to be able to store and manage data for our website in relational database.

$sudo apt install mysql-server

after installation successfull we can log in to mysql

$sudo mysql

![accessing mysql](https://user-images.githubusercontent.com/96633325/225010538-be05e65e-2977-4607-a043-bf5362ce09e9.PNG)

# STEP3 - Installing PHP

we have installed Nginx server and MySQl database to store and manage data. now we can install PHP to proccess code generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a
bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based 
websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”,
and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows 
PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies

$sudo apt install php-fpm php-mysql

# STEP 4 - Configuring NGINX to use PHP processor

create the root web directory for domain 

$ sudo mkdir /var/www/projectLEMP

assign ownership of the directory with the $USER environment variable
$sudo chown -R $USER:$USER /var/www/projectLEMP

open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor

sudo nano /etc/nginx/sites-available/projectLEMP

write in new blank file

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
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory
$sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

now go to the website by using web browser with instance ip address and port 80
![configuring ngnix to use php proccessor and tested](https://user-images.githubusercontent.com/96633325/225015689-c1c9d289-9560-4c2a-8371-4c12525a7079.PNG)

#STEP 5 - RETRIEVING DATA FROM MYSQL
access the mysql database

$ sudo Mysql 

Create database php_database;

then create table named todo_list
CREATE TABLE php_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );

Enter some rows of content in table

INSERT INTO php_database.todolist (content) Values ("php info with nginx");

to confirm data is saved 

SELECT * From php_database.todolist
![verify that my sql working perfect](https://user-images.githubusercontent.com/96633325/225018087-5838ae71-abdc-4aa4-9bad-3733f9c48a0f.PNG)
---------------------------------------------------------------------------------------------------------------------------------------------------








