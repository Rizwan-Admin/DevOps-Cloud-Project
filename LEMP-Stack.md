# WEB STACK IMPLEMENTATION (LEMP STACK)- 101
### Step 0 - PreRequisites 

* aws account 
* ubuntu server = aws ec2 instance --> ubuntu 24.04 LTS (HVM) 
* git bash 

##### Launch Git Bash and run the following command:
```
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-addressa>
```

it will look like this
passte image=========

### Step 1 – Installing the Nginx Web Server
* Update the package index: Run the command 
```
sudo apt update -y
```

* Install Nginx: Run the command
```
sudo apt install nginx
```

* To verify that nginx was successfully installed and is running as a service in Ubuntu, Run the command
```
sudo systemctl status nginx
```
Note : If it is green and running, then you did everything correctly - you have just launched your first Web Server in the Clouds!

* Web server rules :==> The following inbound rules allow HTTP and HTTPS access from any IP address.
![image](https://github.com/user-attachments/assets/71037cf6-6d4e-4bb0-b162-2fb3e36fae2c)

* Note: First, let us try to check how we can access it locally in our Ubuntu shell, run:
  ```
  curl http://localhost:80
  ```
  or
  ```
  curl http://127.0.0.1:80
  ```
  add image ====
  
* Testing Nginx Server Response from the Internet via Browser
```   
http://<Public-IP-Address>:80 
```
add image=====


* Retrieving Public IP Address Using AWS Metadata Service
```
  curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

### Step 2 — Installing MySQL
**Setting Up MySQL for Database Management**

**Project Point:**
* Now that the web server is successfully running, the next step is to install a Database Management System (DBMS) to store and manage data in a relational database. MySQL, a widely-used relational database management system, will be implemented for this project to manage the backend database for the PHP environment.

  Commands:
* Install MySQL Server:

```
sudo apt install mysql-server -y 
```

* Access MySQL
```
sudo mysql
```
* Setting MySQL Root User Password
```
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

* Exit MySQL Shell
```
  exit
```

* Running the MySQL Secure Installation Script
```
sudo mysql_secure_installation
```
* Configuring the VALIDATE PASSWORD PLUGIN
  add image ===

* MySQL Login
```
  mysql -u root -p
```

* exit the Mysql console
```
exit
```
#### Note:
* MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack

### Step 3 – Installing PHP
* Description: With Nginx serving your content and MySQL managing your data, the next step is to install PHP for processing dynamic content. Unlike Apache, which embeds the PHP interpreter in each request, Nginx uses PHP-FPM (FastCGI Process Manager) to handle PHP processing externally. This setup enhances performance but requires additional configuration.

Commands: To install both PHP-FPM and PHP-MySQL, which allows PHP to communicate with MySQL databases, run the following command:

* Installing PHP and PHP-MySQL
```
sudo apt install php-fpm php-mysql
```

