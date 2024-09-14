# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
## Introduction
LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
LAMP is a popular open-source software stack used for web development. It stands for Linux (operating system), Apache (web server), MySQL (database), and PHP, Python, or Perl (programming languages), enabling dynamic website and application development. The stack provides a reliable and flexible foundation for hosting websites and applications.
# Step 0 - Prerequisites:
#### launch a EC2 instance of t2.micro family with Ubuntu Server 24.04 LTS (HVM) in Ohio Region using AWS Console.

![image](https://github.com/user-attachments/assets/4070c5dc-b46f-4ab2-83f2-1a199e4dbbbf)

![image](https://github.com/user-attachments/assets/c528f54d-e145-4af1-90a4-3cf948a17af1)
#### Make sure **Port 22** is open in the security group to allow connections from the SSH client.
![image](https://github.com/user-attachments/assets/5259dd67-bbf6-4af4-93c8-dc87a0694420)

Connect the SSH client via this command:
```
ssh -i "your-key.pem" ubuntu@your-public-ip
```
![image](https://github.com/user-attachments/assets/90b97974-731f-4184-a308-0aaad2139ef9)

# Step 1 — Installing Apache and Updating the Firewall
#### update a list of packages in package manager
```
sudo apt update -y
```
![image](https://github.com/user-attachments/assets/74e8ad8e-c5fd-4665-9128-e8a1b732f882)

#### run apache2 package installation
```
sudo apt install apache2 -y
```
![image](https://github.com/user-attachments/assets/f7c085a7-4fe1-4a00-b77c-0105a358ccc6)

### To verify that apache2 is running as a Service in our OS, use following command
```
sudo systemctl status apache2
```
![image](https://github.com/user-attachments/assets/ca6f3902-fb91-404b-9cc5-541e6356b918)

#### Note: In this project, TCP port 22 is open by default on EC2 instances for SSH access. To allow inbound traffic for HTTP, a security group rule must be added to open port 80.
![image](https://github.com/user-attachments/assets/a5b91076-1dcd-48e1-9cf8-02ed6eeb3200)

### Testing Local Web Server on Port 80 with cURL Command
```
 curl http://localhost:80
 ```
![image](https://github.com/user-attachments/assets/3054de5d-895e-4470-9471-91478a6bba05)

### Accessing the Web Server via Public IP on Port 80
```
http://public-IP-Address:80 
```
![image](https://github.com/user-attachments/assets/934042fc-dc0a-46eb-a2ca-95accf4c37b7)


# Step 2 — Installing MySQL
MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

```
sudo apt install mysql-server
```
![image](https://github.com/user-attachments/assets/23fa4baf-150d-4668-b712-72cd6050f206)

#### Login into  mysql
```
sudo mysql
```
![image](https://github.com/user-attachments/assets/1511b628-8f08-4d03-a143-26e8a3430bba)



## Securing MySQL: Setting Root Password and Running Security Script

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_secure_password';
```
![image](https://github.com/user-attachments/assets/6b68c02e-c391-4fd7-be55-839f03fb2678)

## Exit the mhysql shell

![image](https://github.com/user-attachments/assets/9634fa77-6f2c-45cd-b26c-d079b0ad14cf)

## Start the interactive script by running:
```
sudo mysql_secure_installation
```
![image](https://github.com/user-attachments/assets/18b51076-ee8c-43b3-a994-ec69711f18d9)
## login mysql
```
sudo mysql -p
```
![image](https://github.com/user-attachments/assets/cb0f3303-6aef-4e13-aa60-1b1d55ec97e2)

## exit the mysql
![image](https://github.com/user-attachments/assets/107ed4b2-892a-4baa-b3c3-8f8ced33a7e6)

### Note : MySQL server is now installed and secured.

# Step 3 - Install PHP
You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

 ## To install these 3 packages at once, run:
1- php: The PHP interpreter.

2- libapache2-mod-php: Integrates PHP with the Apache web server.

3- php-mysql: Allows PHP to communicate with MySQL databases.

```
sudo apt install php libapache2-mod-php php-mysql
```
![image](https://github.com/user-attachments/assets/d794081a-a260-4d85-a0ed-1e883b649cc1)


## check php version
```
php -v
```
![image](https://github.com/user-attachments/assets/f025bb3f-d1d2-48f0-96f6-e7136e36ca0d)

## Note: At this point, your LAMP stack is completely installed and fully operational.

# Step 4 — Creating a Virtual Host for your Website using Apache
The default directory serving the apache default page is /var/www/html. Create your document directory next to the default one.

### Create the directory for projectlamp using 'mkdir' command as follows:
```
sudo mkdir /var/www/projectlamp
```
![image](https://github.com/user-attachments/assets/fbf6a326-cd30-46d2-b14f-06dd6b1796c7)

### Assign the directory ownership with $USER environment variable which references the current system user.

```
sudo chown -R $USER:$USER /var/www/projectlamp
```
![image](https://github.com/user-attachments/assets/0959a933-554e-4aea-b836-3d84d334be0b)

###  Create and open a new configuration file in apache’s “sites-available” directory using vi.
```
sudo vim /etc/apache2/sites-available/projectlamp.conf
```
```

<VirtualHost *:80>
  ServerName projectlamp
  ServerAlias www.projectlamp
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/projectlamp
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
![image](https://github.com/user-attachments/assets/197de0c2-0ed6-4130-9d2d-9b4dcc854499)


## Enable the new virtual host
```
sudo a2ensite projectlamp
```
![image](https://github.com/user-attachments/assets/9815db27-f71c-4991-bb04-fd11bfe17718)


### Disable apache’s default website.
```
sudo a2dissite 000-default
```
![image](https://github.com/user-attachments/assets/3c646f54-7124-4a6c-b923-702ad007d001)

### Ensure the configuration does not contain syntax error
```
sudo apache2ctl configtest
```
![image](https://github.com/user-attachments/assets/f6569a8a-5ee0-43a8-9050-25e06d1266c9)



### Reload apache for changes to take effect.
```
sudo systemctl reload apache2
```
![image](https://github.com/user-attachments/assets/6935cb89-aa52-454f-bba5-af08aac9dbac)


### The new website is now active but the web root /var/www/projectlamp is still empty. Create an index.html file in this location so to test the virtual host work as expected.

```
 sudo echo 'Hello LAMP from hostname' \
$(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
&& curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-hostname) \
'with public IP 18.118.158.106' \
> /var/www/projectlamp/index.html
```
![image](https://github.com/user-attachments/assets/f08ed7be-da5a-434e-84cf-2c145443d615)


## Open the website on a browser using the public IP address.
```
http://public-ip-Address:80
```
![image](https://github.com/user-attachments/assets/a95cb660-3b36-4450-9244-df46643de174)

### Open the website with public dns name (port is optional)
```
http://<public-DNS-name>:80
```

![image](https://github.com/user-attachments/assets/72a01d8f-fe70-4c82-8898-8f0700e8526d)


# Step 5 - Enable PHP on the website
### Open the dir.conf file with vim to change the behaviour
```
sudo vim /etc/apache2/mods-enabled/dir.conf
```
```
<IfModule mod_dir.c>
  # Change this:
  # DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
  # To this:
  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
![image](https://github.com/user-attachments/assets/d32a8bda-276e-4aa1-9721-3b887844b186)

### Reload Apache
Apache is reloaded so the changes takes effect.
```
sudo systemctl reload apache2
```
![image](https://github.com/user-attachments/assets/12daa203-e087-4a06-8111-1c3ab3beced3)

### Create a php test script to confirm that Apache is able to handle and process requests for PHP files.
A new index.php file was created inside the custom web root folder.
```
vim /var/www/projectlamp/index.php
```
Add the text below in the index.php file
```
<?php
phpinfo();
```
### Now refresh the page
![image](https://github.com/user-attachments/assets/82115afe-be4a-47cb-9b75-d19661a7e3e0)

This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.
If we can see this page in your browser, then your PHP installation is working as expected.After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:
```
sudo rm /var/www/projectlamp/index.php
```

## Conclusion:
The LAMP stack is a solid foundation for building and deploying web applications. By following this guide, you can easily set up, configure, and maintain a LAMP environment, providing a stable platform for developing scalable web solutions.










