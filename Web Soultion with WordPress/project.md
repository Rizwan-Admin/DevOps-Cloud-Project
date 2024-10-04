# Web Solution With WordPress


In this project tasked to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).


### Two Parts of Project

1. Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give you practical experience of working with disks, partitions and volumes in Linux.

2. Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.

### Three-tier Architechture

Generaally, web or mobile soultion are implemented based on what is called the Three tier Architecture.

Three Tier Architechture is a client- Server sofware architecture pattern that comprise of 3 separate layers.

![image](https://github.com/user-attachments/assets/3c8f1c84-343c-417d-b61c-3b6bcce7556f)



**1. Presentation Layer (PL):** This is the user interfacce such as the client server or brwoser on laptop.

**2. Business Layer (BL):**  This is the backend program that implements business logic.Applicatuoin or Webserver.

**3. Data Access or Managment Layer (DAL):** This is the layer computer mata storage & data access. Databasse server or file System Server sucjh FTP server, or NFFS Server.


**--->** Use **RedHat Os** for this Project

## Step 1 - Prepare a Web Server
1. Launch an EC2 instance --> that will serve as -->**Web server**
   Create 3 volumes in the same AZ as web server EC2 , eacch of 10 GiB.
   Learn How to ADd EBS Voulme to an EC2 Instance https://www.youtube.com/watch?v=HPXnXkBzIHw

   













