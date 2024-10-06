# DevOps Tooling Website Solution
## Introduction
* This project involves implementation of a solution that consists of the following components:
 ```
-Infrastructure: AWS
-Web Server Linux: Red Hat Enterprise Linux 9
-Database Server: Ubuntu Linux + MySQL
-Sotrage Server: Red Hat Enterprise Linux 9 + NFS Server
-Programming Language: PHP
-Code Repository: GitHub
```
-The diagram below shows the architecture of the solution.
<img width="696" alt="image-17" src="https://github.com/user-attachments/assets/c7b5827a-9489-421b-85b2-2bd398fb4d06">


# Step 1 - Prepare NFS Server

Launch new EC2 Instance with RHEL 8 OS.

![image](https://github.com/user-attachments/assets/e1b1078c-7659-4a6b-b2d3-45d7a74d80a8)

connect via ssh

![image](https://github.com/user-attachments/assets/6cbdaeb6-b76b-4ca6-9ede-017fd3156287)

* Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs


![image](https://github.com/user-attachments/assets/f90a9fb2-4b2c-420e-aab0-6025d1f28f27)

* Update mMchine:
```
sudo yum update -y
```
List the disk
```
lsblk
```
![image](https://github.com/user-attachments/assets/70b2c998-7b63-4f76-b63d-ad4e7c376922)

After attached volumes


![image](https://github.com/user-attachments/assets/59c00222-fbc8-4f5e-b0ac-c1b48c053553)


Use df -h command to see all mounts and free space

```
df -h
```
![image](https://github.com/user-attachments/assets/d74c7439-447c-4712-b2c0-271fd9f92bf9)

* Use gdisk utility to create a single partition on each of the 3 disks:
  
```
sudo gdisk /dev/xvdbf
```

![image](https://github.com/user-attachments/assets/b7a6f5fa-f563-4533-8917-ae7be44dd04b)
we follow the same steps for remaining two and create partision:

![image](https://github.com/user-attachments/assets/85bdea48-8c76-4b89-9144-27dc38165996)

* Install lvm2 package: creating logical volumes, which can be resized or moved without needing to unmount file systems.

```
sudo yum install lvm2 -y
```
![image](https://github.com/user-attachments/assets/1564a36e-c981-4e47-87d4-866b16bc0fa9)


Check for available partitions.
```
sudo lvmdiskscan
```

![image](https://github.com/user-attachments/assets/e165f745-49c7-4c8c-8cca-64523c334fff)

Create Physical Volumes Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdbb1 /dev/xvdbc1 /dev/xvdbd1
```
![image](https://github.com/user-attachments/assets/719e36b4-dc49-453b-9fff-7f8d96a61c02)


* Verify that your Physical volume has been created successfully

```
sudo pvs
```
![image](https://github.com/user-attachments/assets/a42f646d-c21b-4436-93f5-ab245bf2e102)


* Use vgcreate utility to add all 3 PVs to a volume group (VG) Name the VG webdata-vg


```
sudo vgcreate webdata-vg /dev/xvdbb1 /dev/xvdbc1 /dev/xvdbd1
```

![image](https://github.com/user-attachments/assets/c6a0eea6-d4ca-4aba-846b-1ea0ce0e7f0b)


Verify that your VG has been created successfully

```
sudo vgs
```
![image](https://github.com/user-attachments/assets/824dbdc0-76b5-4dee-8903-8a9ef95f1974)


* Create Logical Volumes Use lvcreate utility to create logical volumes
```
sudo lvcreate -L 14G -n lv-apps webdata-vg
```
```
sudo lvcreate -L 14G -n lv-logs webdata-vg
```
```
sudo lvcreate -L 14G -n lv-opt  webdata-vg
```


![image](https://github.com/user-attachments/assets/4f00fe73-dc5a-4b8a-b26f-2727e0b662d1)

Verify that our Logical Volume has been created successfully

```
sudo lvs
```
![image](https://github.com/user-attachments/assets/293c6a0f-09a4-487a-b63c-8d19c13d321c)

* Verify the entire setup #view complete setup - VG , PV, and LV
```
sudo vgdisplay -v
```
Instead of formatting the disks as ext4 you will have to format them as xfs.
* Ensure there are 3 Logical Volumes lv-opt lv-apps, and lv-logs
```
sudo lsblk
```
![image](https://github.com/user-attachments/assets/9760b240-a05e-4664-961c-1f5cc8d4ada1)

Format the Logical Volumes as XFS:
```
sudo mkfs.xfs /dev/webdata-vg/lv-apps
```
![image](https://github.com/user-attachments/assets/9376ebe2-7acd-421c-9e14-0b53cc29dfb9)

```
sudo mkfs.xfs /dev/webdata-vg/lv-logs
```
![image](https://github.com/user-attachments/assets/f3f91bf7-0c38-4e31-ba5d-92bd87de78a1)

```
sudo mkfs.xfs /dev/webdata-vg/lv-opt
```
![image](https://github.com/user-attachments/assets/197535a7-986d-4eb0-bd64-bcdd6a7fc35a)


* Create mount points on /mnt directory for the logical volumes as follows: Mount lv-apps on
  
/mnt/apps - To be used by webservers Mount lv-logs on /mnt/logs - To be used by webserver logs Mount lv-opt on /mnt/opt - To be used by Jenkins server in Project 8.

```
 sudo mkdir /mnt/apps
```
```
 sudo mkdir /mnt/logs
```
```
sudo mkdir /mnt/opt
```
![image](https://github.com/user-attachments/assets/0a5ffcf8-05fa-4ed4-97fb-23c898e72a94)


Mount Logical Volumes
```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
```
```
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
```
```
sudo mount /dev/webdata-vg/lv-opt /mnt/opt
```

![image](https://github.com/user-attachments/assets/c968a004-daf7-498c-bb44-e91f13f88a78)


```
lsblk
```
![image](https://github.com/user-attachments/assets/fca8bd27-d867-4371-9996-1ad9091d3c06)


Add Mount Points to /etc/fstab

```
sudo vi /etc/fstab
```

Add the following lines:

```
/dev/webdata-vg/lv-apps /mnt/apps xfs defaults 0 0
/dev/webdata-vg/lv-logs /mnt/logs xfs defaults 0 0
/dev/webdata-vg/lv-opt /mnt/opt xfs defaults 0 0
````


Verify Mounts:

```
sudo mount -a
```

* Install NFS server, configure it to start on reboot and make sure it is u and running. Update the System and Install NFS Utilities:


```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![image](https://github.com/user-attachments/assets/6704d8e2-17f1-483b-a396-92654d94c785)



4.Export the NFS Mounts Use subnet cidr to connect as clients. For simplicity, we will install our all three Web Servers inside the same subnet, but in production set up would probably want to separate each tier inside its own subnet for higher level of security. To check our subnet cidr - open our EC2 details in AWS web console and locate Networking tab and open a Subnet link:

Set Permissions on Mount Points.


```
sudo chown -R nobody:nobody /mnt/apps
sudo chown -R nobody:nobody /mnt/logs
sudo chown -R nobody:nobody /mnt/opt
sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

```

Restart NFS Server

```
sudo systemctl restart nfs-server.service
```
Configure access to NFS for clients within the same subnet (example of Subnet CIDR - 172.31.32.0/20 ):

```
sudo nano /etc/exports
```
Add the following lines:

```
/mnt/apps 172.31.0.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.0.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.0.0/20(rw,sync,no_all_squash,no_root_squash)
```
note :save and exit from the editor by ctrl+s ctrl+x
![image](https://github.com/user-attachments/assets/ba0e873b-c7fc-49c0-9763-da83b797c902)


* Export the NFS Shares:

```
sudo exportfs -arv
```
![image](https://github.com/user-attachments/assets/ab0b6d08-47f7-4a1d-8d27-faecc054d6d7)


* Check which port is used by NFS and open necessary ports in Security Groups (add new Inbound Rule)
* Check NFS Ports:

```
rpcinfo -p | grep nfs
```
![image](https://github.com/user-attachments/assets/661bf62a-cd31-4185-8afe-a58b8fef583e)


* Important note: In order for NFS server to be accessible from our client,we open following ports:
TCP 111
UDP 111
UDP 2049


Step 2 — Configure the database server
By now you should know how to install and configure a MySQL DBMS to work with remote Web Server


Install MySQL server
Create a database and name it tooling
Create a database user and name it webaccess
Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr

![image](https://github.com/user-attachments/assets/982e0cab-4a85-47fa-b6f0-7f3d87f6cd64)


```
sudo apt update -y
```

Install MySQL server:

```
sudo apt install mysql-server
```

Check status:

```
sudo systemctl status mysql
```

Enable MySQL to start on boot:

```
sudo systemctl enable mysql
```


* Create a database Tooling and User name it webaccess.
```
sudo mysql
```

```
CREATE DATABASE tooling;
```

```
CREATE USER 'webaccess'@'%' IDENTIFIED BY 'mypass';
```

```
GRANT ALL PRIVILEGES ON tooling. * TO 'webaccess'@'%' WITH GRANT OPTION;
```

```
exit
```
![image](https://github.com/user-attachments/assets/4477c5a5-9b36-4418-8eea-ed831eb32383)

## Step 3 - Prepare the Web Servers

In this step we will do the following:

* Configure NFS client (this step must be done on all three servers).
* Deploy a Tooling application to our Web Servers into a shared NFS folder.
* Configure the Web Servers to work with a single MySQL database For server One.
1. Launch a new EC2 instance with REDHAT Operating System.


Updating machine...


```
sudo yum update -y
```

Installing NFS CLIENT:

```
sudo yum install nfs-utils nfs4-acl-tools -y
```


2. Mount /var/www/ and target the NFS server's export for apps

```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid 172.31.15.210:/mnt/apps /var/www
sudo mount -t nfs -o rw,nosuid 172.31.33.253:/mnt/apps /var/www/
```

* Verify that NFS was mounted successfully

```
 df -h
```
* Make sure that the changes will persist on Web Server after reboot:
```
sudo vi /etc/fstab
```

add following line
```
<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0
```
172.31.15.210:/mnt/apps /var/www nfs defaults 0 0

![image](https://github.com/user-attachments/assets/7f78d125-29fa-46c7-a703-87131ee0a05d)

Install Remi's repository, Apache and PHP

```
sudo yum install httpd -y
sudo systemctl enable httpd.service
sudo systemctl start httpd.service
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo dnf module reset php
sudo dnf module enable php:remi-7.4
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo setsebool -P httpd_execmem 1
```

Repeat steps 1-5 for another 2 Web Servers.

Verify NFS Mount and Apache Setup: Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. If you see the same files - it means NFS is mounted correctly.

```
cd /var/www
```
You can try to create a new file.
```
sudo touch test.txt
```

Fork the tooling source code from Darey.io Github Account to your Github account. Download git.

```
sudo yum install git -y
```
```
sudo git clone https://github.com/Rizwan-Admin/tooling.git
```

![image](https://github.com/user-attachments/assets/b0ae39a6-a627-418c-bdd6-1320bd7dfe35)

* Deploy the tooling website's code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html
   Note 1: Do not forget to open TCP port 80 on the Web Server.
   Note 2: If you encounter 403 Error - check permissions to your /var/www/html folder and also disable SELinux sudo setenforce 0 To make this change permanent - open following config file


* Disable SELinux

```
sudo setenforce 0
sudo vi /etc/sysconfig/selinux
```

and set SELINUX=disabled
![image](https://github.com/user-attachments/assets/f5a056f0-7427-48df-a760-c1c039996da7)


Then restrt httpd

```
sudo systemctl restart httpd
sudo systemctl status httpd
```

Update the website's configuration to connect to the database (in /var/www/html/functions.php file). Apply tooling-db.sql script to your database using this command.

Create in MySQL a new admin user with username: myuser and password: password:

Creating user and assignning the Required All Permission

```
CREATE USER 'admin'@'172.31.15.211' IDENTIFIED BY 'admin';
```

```
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'172.31.15.211' WITH GRANT OPTION;
```

```
FLUSH PRIVILEGES;
```

Use databases;
```
USE tooling

```
Creating the table


```
CREATE TABLE users (
id INT PRIMARY KEY,
username VARCHAR(50),
password VARCHAR(100),
email VARCHAR(100),
user_type VARCHAR(20),
status TINYINT(1)
     );
```

```
INSERT INTO `users` (`id`, `username`, `password`, `email`, `user_type`, `status`) 
VALUES (1, 'myuser', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');
```


* Open the website in your browser http://Web-Server-Public-IP-Addressor Public-DNS-Name/index.php and make sure you can login into the website with your user and password.
```
http://public-ip-address/login.php
```
![image](https://github.com/user-attachments/assets/bf15f470-f237-4ac0-8fe8-081e281a2ef4)
![image](https://github.com/user-attachments/assets/98a6915d-d204-4d59-b6c0-6b8a91159954)

### Conclusion

In implementing a web solution for the DevOps team using the LAMP stack, we successfully integrated a remote MySQL database and NFS servers to enhance the application’s performance and scalability. This architecture not only streamlines data access and management but also ensures efficient file sharing across the team. By leveraging these technologies, we have established a robust framework that supports the dynamic needs of the DevOps environment, facilitating seamless collaboration and increased productivity. This project showcases our commitment to employing modern practices in cloud engineering to deliver efficient and scalable solutions.
