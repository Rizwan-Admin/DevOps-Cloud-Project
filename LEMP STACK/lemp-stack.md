# WEB STACK IMPLEMENTATION (LEMP STACK)- 101

* The LEMP project integrates Nginx as the web server, MySQL as the database management system, and PHP for dynamic content generation, providing a high-performance and scalable environment for web applications. This setup ensures efficient resource management and a seamless user experience.

Step 0 - Preparing prerequisites
* Ubuntu Server 24.04 LTS (HVM) on AWS
* Git Bash

![image](https://github.com/user-attachments/assets/53521dd8-e872-4c53-97ae-95966a7f370d)

![image](https://github.com/user-attachments/assets/c4099a26-3d84-4d60-b18f-c3132a8d5ce0)

* Launch Git Bash and run the following command:
```
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```
It will look like this:
![image](https://github.com/user-attachments/assets/f28dde90-6de5-410c-8c53-22b7bcd60d15)

Step 1 – Installing the Nginx Web Server
```
 sudo apt update -y
```
```
 sudo apt install nginx -y 
```

* To verify that nginx was successfully installed and is running as a service in Ubuntu

```
sudo systemctl status nginx
```

![image](https://github.com/user-attachments/assets/a10a84be-ab8f-4289-b381-f0ab8068fc60)

* If it is green and running, then you did everything correctly - you have just launched your first Web Server in the Clouds!

###  open TCP port 80 which is default port that web brousers use to access web pages in the Internet.
![image](https://github.com/user-attachments/assets/42273341-5a10-4b85-a6bb-c840bb7d3bfc)


* First, let us try to check how we can access it locally in our Ubuntu shell,
```
curl http://localhost:80
```

```
curl http://127.0.0.1:80
```
![image](https://github.com/user-attachments/assets/2e3558cd-a4d2-4605-8432-d98469d301bf)


* Now it is time for us to test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and try to access following url
```
http://<Public-IP-Address>:80
```

![image](https://github.com/user-attachments/assets/be7c82b2-43d0-44ed-acb2-73d7fb198cf9)


Step 2 — Installing MySQL

```bash
sudo apt install mysql-server
```
To Check mysql status
```bash
sudo systemctl status mysql
```

![image](https://github.com/user-attachments/assets/2e202977-231a-4072-99f9-3cac0c1779a5)

```bash
sudo mysql
```
![image](https://github.com/user-attachments/assets/beebe879-3ff8-4db8-98d7-dc9b17485ab7)


```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Passwd@1';
```
* exit mysql shell
```
exit
```
![image](https://github.com/user-attachments/assets/47e0d501-f3b8-4c8a-8118-bf159454c088)


* Securing MySQL:

```
sudo mysql_secure_installation
```
![image](https://github.com/user-attachments/assets/fdb825cc-fb47-4fa9-b71c-a7ec73456e55)


* Login into Mysql:
```
sudo mysql -p
```
![image](https://github.com/user-attachments/assets/7a407818-7622-4b58-a4bd-f94dc4b45aab)

* Exit to mysql
```
exit
```
* MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack.

Step 3 – Installing PHP

* To install these 2 packages at once, run:

```
sudo apt install php-fpm php-mysql
```

* To check php version

```
php -v
```
![image](https://github.com/user-attachments/assets/b80a8262-c605-4bbf-b841-c7e3101cc0f5)

* now  PHP components installed. Next,  configure Nginx.


Step 4 — Configuring Nginx to Use PHP Processor

* When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

* On Ubuntu 24.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

Create the root web directory for domain as follows


```
sudo mkdir /var/www/CloudTimes
```

* Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

  ```
  sudo chown -R $USER:$USER /var/www/CloudTimes
  ```
  * Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
 
    ```
    sudo nano /etc/nginx/sites-available/CloudTimes
    ```
* This will create a new blank file. Paste in the following bare-bones configuration:
```
#/etc/nginx/sites-available/CloudTimes
server {

      listen 80;
      server_name cloudtimes www.cloudtimes;
      root /var/www/CloudTimes;

      index index.html index.htm index.php;

      location / {
              try_files $uri $uri/ =404;

      }

      location ~ \.php$ {
              include snippets/fastcgi-php.conf;
              fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;

      }

      location ~ /\.ht {
              deny all;
      }
 }
```
![image](https://github.com/user-attachments/assets/517d1d32-98f2-4c90-b8f1-b14a7f27f662)


* Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

  ```
  sudo ln -s /etc/nginx/sites-available/CloudTimes /etc/nginx/sites-enabled/
  ```
  ![image](https://github.com/user-attachments/assets/a04dbb60-726f-4e89-bfea-7d9ddd9975e1)

* Checking the syntax of Configuartion file.

```
sudo nginx -t
```
![image](https://github.com/user-attachments/assets/d7cbfa14-4a6b-4b66-87cb-c88895cf1645)

*  Disable default Nginx host that is currently configured.
  ```
sudo unlink /etc/nginx/sites-enabled/default
```

* Reloading nginx so that it will reload the configuration which we configure so far

```
sudo systemctl reload nginx
```
* Create an index.html file in that location so that we can test that your new server block works as expected:

```
sudo echo 'Hello LEMP from hostname ' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-hostname) ' with public IP ' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/CloudTimes/index.html
```
![image](https://github.com/user-attachments/assets/e9470ffb-0a8d-4ae1-b7d4-a1c4703b8cea)

* Check into Browser using IP Address
  ![image](https://github.com/user-attachments/assets/f5fd1d4f-9f69-43b8-a319-6867dc269f94)

Step 5 – Testing PHP with Nginx
* At this point, LEMP stack is completely installed and fully operational. We can test it to validate that Nginx can correctly handle .php files off to PHP processor. We can do this by creating a test PHP file in document root. Open a new file called info.php within your document root.

  ```
  nano /var/www/CloudTimes  /info.php
  ```
 * Writing PHP script inside the info.php
```
<?php
phpinfo();
?>
```
![image](https://github.com/user-attachments/assets/6432cfe8-b228-4bb6-b9ef-3e0238574372)

* Accessing the PHP Website

```
http://`server_domain_or_ip`/info.php
```


![image](https://github.com/user-attachments/assets/628533bc-ca52-447f-9397-50e9cf01efc0)

* After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:

```
sudo rm /var/www/CloudTimes/info.php
```
![image](https://github.com/user-attachments/assets/52354f4c-cabb-4dbe-a754-ca5f5fbe9d0d)

  

Step 6 — Retrieving data from MySQL database with PHP

* we will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.
At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.
We will create a database named tester_db and a user named tester_user,

* First, connect to the MySQL console using the root account:
  ```
  sudo mysql -p
  ```
  ![image](https://github.com/user-attachments/assets/fc7d2029-18fc-43ec-a0f4-d9c0ffbc1678)

* create a new database
```
CREATE DATABASE `example_database`;
```
![image](https://github.com/user-attachments/assets/613a255d-312b-4cf1-82db-680966c6d20b)


* create a new user and grant full privileges on the database ,just created.

```
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Passwd@1';
```
![image](https://github.com/user-attachments/assets/c3d5b851-4ea4-47cd-a5b1-2a3d7eb13af3)


* Granting the privileges to example_user for example_db
```
GRANT ALL ON example_database.* TO 'example_user'@'%';
```
![image](https://github.com/user-attachments/assets/a1aa62c7-e283-4efc-bee9-21ad4d4583ee)


exit mysql console
```
exit
```

* Test if the new user has the proper permissions by logging in to the MySQL console again.

```
mysql -u example_user -p
```
![image](https://github.com/user-attachments/assets/02eac0d9-7669-418c-a998-77bc9c36754a)


* check availabe databse
  ```
  show databases;
  ```


* create a test table named todo_list. From the MySQL console:

```
CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY (item_id)
);
```
![image](https://github.com/user-attachments/assets/d63fc4d3-8ac1-48fe-95e4-b27c6fd0a0d8)


* Inserting a few rows of content in the test table

  ```
  INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
  ```
  ![image](https://github.com/user-attachments/assets/a5a79e70-265b-47b7-b0fc-a97d67c85e28)
```
INSERT INTO example_database.todo_list (content) VALUES ("My 2nd important item");
```

```
INSERT INTO example_database.todo_list (content) VALUES ("My 3rd important item");
```



* To confirm that the data was successfully saved to your table, run:
  ```
  SELECT * FROM example_database.todo_list;
  ```
![image](https://github.com/user-attachments/assets/7560d09f-50b2-4eb2-95d9-7555ade4c257)


* Now we can create a PHP script that will connect to MySQL and query for content

```
nano /var/www/CloudTimes/todo_list.php
```


```
<?php
$user = "example_user";
$password = "Passwd@1";
$database = "example_database";
$table = "todo_list";

try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);

    echo "<h2>TODO</h2><ol>";
    foreach ($db->query("SELECT content FROM $table") as $row) {
        echo "<li>" . htmlspecialchars($row['content']) . "</li>";
    }
    echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}


```

![image](https://github.com/user-attachments/assets/991b46c1-f1be-4a3f-b76d-3f7350bda6f6)


* Now accessing this page in web browser by visiting the domain name or public IP

```
http://ip-address/todo_list.php
```

![image](https://github.com/user-attachments/assets/0813e89b-d66c-4dd1-aa60-7c66073d6be1)

* Note : That means your PHP environment is ready to connect and interact with your MySQL server.

 ### Conclusion

* In this project, we utilized Nginx as the web server and MySQL as the database management system to create a robust platform for serving PHP websites and applications. This setup offers high performance, scalability, and efficient resource management, ensuring a reliable experience for users. With this solid foundation, you are well-prepared to develop and expand your web projects effectively.




