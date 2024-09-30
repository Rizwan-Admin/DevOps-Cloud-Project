# Client-Server Architecture with MySQL

* ## Understanding Client-Server Architecture

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
* Provisioning a New Two EC2 instance of t2.micro family with Ubuntu Server 24.04 LTS (HVM):
```
Server A name - `mysql server`
Server B name - `mysql server`
```
![image](https://github.com/user-attachments/assets/4bb754c6-ad69-48ee-ade2-7ff92cc24ccd)
![image](https://github.com/user-attachments/assets/e6b4ba71-0ad5-4953-9d52-1ee626018e2b)



* Connect to your instance via ssh

  ![image](https://github.com/user-attachments/assets/134d7c55-1789-49b9-a5f1-4d6e64636318)


* Update and Upgrading packages in both instances:
  ```
  sudo apt update && sudo apt upgrade -y
  ```
  ![image](https://github.com/user-attachments/assets/a4d265f0-4fee-48f9-be0d-15a90955dbc6)


* Edit the mysql configuration file to allow remote connections.
  ```
  sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
  ```

  
  


