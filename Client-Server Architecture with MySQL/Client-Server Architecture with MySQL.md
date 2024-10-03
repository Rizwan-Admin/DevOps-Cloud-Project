
* ## Client-Server Architecture

  * Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.
 
  * n their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".
  * A simple diagram of Web Client-Server architecture is presented below.
 
  ![image](https://github.com/user-attachments/assets/6315b41a-3be8-48bb-9db3-0a5fc79ec353)
In the example above, a machine that is trying to access a Web site using Web browser or simply 'curl' command is a client and it sends HTTP requests to a Web server (Apache, Nginx, IIS or any other) over the Internet.

If we extend this concept further and add a Database Server to our architecture, we can get this picture:

![image](https://github.com/user-attachments/assets/a64c84d6-bc30-432c-80f0-2bcb6e8d6fe3)


In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

* Today, we will be implementing a server-client architecture using MYSQL database managemet system on aws EC2 instance.

# Steps involved:
1. Create and configure two Linux-based virtual servers (EC2 instances in AWS).
```
Server A name - `mysql server`
```

```
Server B name - `mysql server`
```
![image](https://github.com/user-attachments/assets/4bb754c6-ad69-48ee-ade2-7ff92cc24ccd)



![image](https://github.com/user-attachments/assets/96b37b8f-1b13-4636-9d37-0bc3715e5fbe)


=======================


### Step 1: Launch EC2 Instances

![image](https://github.com/user-attachments/assets/bc29380a-0f5f-4893-a8ca-44aa38123a59)













### Step 2: Configure the MySQL Server on Server-A 

1. SSH into Server-A (MySQL Server):
```
ssh -i "my-ec2-key.pem" ubuntu@p<ublic-ip-address>
```
![image](https://github.com/user-attachments/assets/32d55716-1bbc-4507-b7df-8bc4da0de6e7)



2. Update and Upgrade the Ubuntu System:
```
sudo apt update && sudo apt upgrade -y
```
![image](https://github.com/user-attachments/assets/53dfc718-b28c-4827-a789-dd6430eaece1)

3. Install MySQL Server:
```
sudo apt install mysql-server -y
```
![image](https://github.com/user-attachments/assets/d84f8eb4-2bdb-449a-a4db-956438801659)

4. Enable MySQL to Start on Boot:
```
sudo systemctl enable mysql
```
![image](https://github.com/user-attachments/assets/8029dea4-5b36-4294-b228-3afacd031673)


5. Secure MySQL Installation:

```
sudo mysql_secure_installation
```
![image](https://github.com/user-attachments/assets/a82ee603-d753-4975-9770-1ed1ea3c79bd)

![image](https://github.com/user-attachments/assets/4cfa8b8e-a23c-4e74-bd64-fba625f374f8)

6. Access MySQL Shell:

```
sudo mysql
```
![image](https://github.com/user-attachments/assets/64e7b9a6-135c-467a-8a81-cc7a4117fabf)

7. Create a User and Database for the Client:

```
CREATE USER 'client'@'%' IDENTIFIED WITH mysql_native_password BY 'Client123$';
CREATE DATABASE test_db;
GRANT ALL ON test_db.* TO 'client'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
![image](https://github.com/user-attachments/assets/e901a6de-ceef-4669-87ed-8bb1b83a81c7)



8. Allow Remote Connections:

Edit the MySQL configuration file to bind to all network interfaces:

```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

Change the line:
```
bind-address = 127.0.0.1
```
To
```
bind-address = 0.0.0.0
```
![image](https://github.com/user-attachments/assets/20f7c899-d55b-4343-be5f-5a989f799af8)

9. Restart MySQL Service:
```
sudo systemctl restart mysql
```
![image](https://github.com/user-attachments/assets/e3d7000d-7de3-4bf3-87f6-65aa499253dc)

### Step 3: Configure the MySQL Client on Server-B
1. SSH into Server-B (MySQL Client)

```
ssh -i "my-ec2-key.pem" ubuntu@<public-ip-address>
```


![image](https://github.com/user-attachments/assets/0d388232-238b-4e2c-8d42-9827ae36aa71)

2. Update and Upgrade the Ubuntu System:

```
sudo apt update && sudo apt upgrade -y
```
![image](https://github.com/user-attachments/assets/4b679c8c-40b5-403e-ac5c-6d8555ad7a24)

3. Install MySQL Client:

```
sudo apt install mysql-client -y
```
![image](https://github.com/user-attachments/assets/0953e459-dc93-48a5-85fa-b183bdac6175)



### Step 4: Configure Networking
1. Open MySQL Port 3306 on Server-A Security Group:
In AWS, modify the Security Group for Server-A (MySQL Server) to allow inbound traffic on port 3306 from the private IP address of Server-B (172.31.12.16).

2. Verify the Inbound Rule:
Ensure that Server-A has the following rule in its security group:

Type: MySQL/Aurora
Protocol: TCP
Port Range: 3306
Source: 172.31.12.16/32 (Server-B private IP)

![image](https://github.com/user-attachments/assets/cd1175c3-47d6-4f9e-b1ac-423b59d06fa7)



Step 5: Connect MySQL Client to MySQL Server

1. From Server-B (MySQL Client), Connect to the MySQL Server Using the Private IP of Server-A:

```
mysql -u client -h 172.31.0.254 -p
```

![image](https://github.com/user-attachments/assets/69f7d3d2-c2d1-4738-a7ae-404447aabcd4)





2. Verify the Connection by Listing the Databases:

```
SHOW DATABASES;
```

![image](https://github.com/user-attachments/assets/85d488f1-69ee-46c7-b907-e1908e31068d)



```
INSERT INTO test_table (content) VALUES ('My first choice football club is Chelsea');
INSERT INTO test_table (content) VALUES ('My second choice football club is R.Madrid');
```


![image](https://github.com/user-attachments/assets/15ea9ec9-f37e-4860-881b-0c52bd8df431)



2. Select Data from the Table:

![image](https://github.com/user-attachments/assets/2ed32533-1316-4ee4-abe3-240d165ae807)



Here is the revised **Step 6** with more detailed SQL operations, keeping it concise:

---




```sql
SHOW DATABASES;
```
![image](https://github.com/user-attachments/assets/056e510c-b4c6-4b37-b959-704959b99365)


#### 2. **Use the `test_db` Database:**
```sql
USE test_db;
```
![image](https://github.com/user-attachments/assets/d3dd622c-3788-4c71-9724-273089f0bae7)

#### 3. **Create a Table in `test_db`:**
```sql
CREATE TABLE test_table (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY (item_id)
);
```

#### 4. **Insert Data into `test_table`:**
```sql
INSERT INTO test_table (content) VALUES ('My first choice football club is Chelsea');
INSERT INTO test_table (content) VALUES ('My second choice football club is R.Madrid');
```
![image](https://github.com/user-attachments/assets/5901937a-c39a-4798-a4b9-f1a316bd4444)

#### 5. **Select Data from `test_table`:**
```sql
SELECT * FROM test_table;
```
![image](https://github.com/user-attachments/assets/93b34723-2409-4bf8-900f-177bbd24a2fd)




### Conclusion

In this project, we successfully implemented a **MySQL Client-Server architecture** using two EC2 instances: **Server-A (MySQL Server)** and **Server-B (MySQL Client)**. The process involved setting up MySQL on Server-A, configuring remote access, and installing the MySQL client on Server-B. Networking was secured by allowing only the private IP of Server-B to connect to Server-A.

We tested the architecture by connecting to the MySQL server from the client, creating a database, performing SQL operations, and verifying the functionality through database queries. This deployment is now fully functional, enabling efficient client-server communication for database management.


























