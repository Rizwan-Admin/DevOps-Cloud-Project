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
1. **Launch an EC2 instance --> that will serve as --> Web server**

   Learn How to ADd EBS Voulme to an EC2 Instance https://www.youtube.com/watch?v=HPXnXkBzIHw
![image](https://github.com/user-attachments/assets/71b95c6d-2745-4103-ba38-35fad934be44)

* Connect instance via ssh & run ``` lsblk ```
  ![image](https://github.com/user-attachments/assets/5759064c-ba06-4908-b2f7-a98f7548c6aa)


*    **Create 3 volumes in the same AZ as web server EC2 , eacch of 10 GiB**
![image](https://github.com/user-attachments/assets/87af0680-01b1-49f6-aeb5-6b8633ef7e92)



* Attach all three volumes one by one to your Web Server EC2 instance.
* use lsblk command to inspect what block are attached to the server

  ```
  lsblk
  ````
  ![image](https://github.com/user-attachments/assets/c3606bb4-acdb-4f36-945e-2f4df134990d)



* use df -h command to see all mount and free space on server
  ```
  df -h
  ```
  
![image](https://github.com/user-attachments/assets/58700d14-27f1-48fe-b8bd-ec861910327c)





* Use gdisk utility to create a single partition on each of the 3 disks
```
sudo gdisk /dev/xvdbf
```
![image](https://github.com/user-attachments/assets/738028a7-694b-4575-93fb-13a72b663733)


```
sudo gdisk /dev/xvdbg
```
```
sudo gdisk /dev/xvdbh
```
* Use lsblk utility to view the newly configured partition on each of the 3 disks.
```
lsblk
```
![image](https://github.com/user-attachments/assets/0511b5b2-887b-4018-be5f-107d7de24b29)



* Install lvm2 package. Lvm2 is used for managing disk drives and other storage devices

```
sudo yum install lvm2
```
![image](https://github.com/user-attachments/assets/ea8ff5a2-6f34-42b2-9c9c-b5d6c64e2246)

![image](https://github.com/user-attachments/assets/1ff24dd6-3d83-49b8-875c-0aeb59a73116)


* Run sudo lvmdiskscan to check for available partitions.
  
```
sudo lvmdiskscan
```
![image](https://github.com/user-attachments/assets/419fccfd-a995-480d-b797-851d98c0da8d)


* Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM.
```
sudo pvcreate /dev/xvdbf1
sudo pvcreate /dev/xvdbg1
sudo pvcreate /dev/xvdbh1
```
![image](https://github.com/user-attachments/assets/485e969f-c70b-495c-8758-b5af1b53efdc)

* Verify that your Physical volume has been created successfully by running sudo pvs
  ```
  sudo pvs
  ```

![image](https://github.com/user-attachments/assets/9e91f24a-a724-4779-ac3b-fdb5c5bf988e)

* Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg

```
sudo vgcreate webdata-vg /dev/xvdbh1 /dev/xvdbg1 /dev/xvdbf1
```
![image](https://github.com/user-attachments/assets/554d3fe8-20b2-4898-827c-22b4553d5104)

* Verify that your VG has been created successfully by running sudo vgs

```
sudo vgs
```
![image](https://github.com/user-attachments/assets/27c60511-18fa-4aad-9204-67837c2a4684)




* Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.


```
sudo lvcreate -n apps-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/0ee57c8a-4a66-459c-8478-67da7ffe2252)


```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/7896a82a-c171-4f37-a59c-bb2ccdb18b75)



* Verify that your Logical Volume has been created successfully by running sudo lvs

```
sudo lvs
```
![image](https://github.com/user-attachments/assets/8b3c2764-1ffa-43d1-9d3f-f679d2f8a3b2)


* Verify the entire setup
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
```

![image](https://github.com/user-attachments/assets/879b2c24-c7a6-4214-b33d-ea738c272078)

```
sudo lsblk 
```
![image](https://github.com/user-attachments/assets/39a09138-b62e-497a-a93d-a733358ad058)


* Use mkfs.ext4 to format the logical volumes with ext4 filesystem
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```
![image](https://github.com/user-attachments/assets/d3d56936-6b86-40e5-bb68-010896e1b481)


```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](https://github.com/user-attachments/assets/aee45f93-efcb-4f6c-8515-3e156fd9a21c)


* Create /var/www/html directory to store website files sudo mkdir -p /var/www/html

```
sudo mkdir -p /var/www/html
```
 * Create /home/recovery/logs to store backup of log data sudo mkdir -p /home/recovery/logs
```
sudo mkdir -p /home/recovery/logs
```


* Mount /var/www/html on apps-lv logical volume
```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/

```

* Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
```
sudo rsync -av /var/log/ /home/recovery/logs/
```
![image](https://github.com/user-attachments/assets/d2a5c111-c230-4c5b-be88-9621c53785db)




* Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)

```
sudo mount /dev/webdata-vg/logs-lv /var/log

```
* Restore log files back into /var/log directory
```
sudo rsync -av /home/recovery/logs/ /var/log
```

![image](https://github.com/user-attachments/assets/dead1f14-9a08-40bd-8f09-3a56f44f7f23)



* Update /etc/fstab file so that the mount configuration will persist after restart of the server.
The UUID of the device will be used to update the /etc/fstab file

```
sudo blkid

```
![image](https://github.com/user-attachments/assets/154f144d-57db-4517-908d-54184900a600)


UUID=29d40505-379a-485c-84db-977508c1723a

UUID=bdd6001b-4ed7-440f-9fc6-273d18382db0



```
sudo vi /etc/fstab
```

![image](https://github.com/user-attachments/assets/259c503c-b238-49ea-b998-f37d5999e7b2)




 * Test the configuration and reload the daemon

```
sudo mount -a
```
![image](https://github.com/user-attachments/assets/58db55c1-e709-474e-9b88-8a20d7f517c6)




```
sudo systemctl daemon-reload
```
![image](https://github.com/user-attachments/assets/adfa010b-55d6-4392-bb3d-2d4c9589f384)





* Verify your setup by running df -h, output must look like this:
```
df -h
```
![image](https://github.com/user-attachments/assets/00970de6-32d1-4ae4-bbe7-d6e203e84e30)


## Step 2- Prepare Database Server
* Launch Instamce:
![image](https://github.com/user-attachments/assets/25a6d48c-f337-4700-8595-079ecbf81f45)

connect via ssh


![image](https://github.com/user-attachments/assets/77c4e19a-1516-47f9-b5c5-10270317b9b9)




## Step 3 — Install WordPress on your Web Server EC2



* Update the repository
```
sudo yum -y update
```


* Install wget, Apache and it's dependencies
```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![image](https://github.com/user-attachments/assets/8f797b12-2844-4d62-9a15-0b338421df80)



* Start Apache
```
sudo systemctl enable httpd
```
![image](https://github.com/user-attachments/assets/f14cc71d-f1cd-4035-9e3b-f9fb23e21b92)
```
sudo systemctl start httpd
```
![image](https://github.com/user-attachments/assets/648673cd-c401-4147-af28-bf67f73af60d)



* To install PHP and it's dependencies
```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```
![image](https://github.com/user-attachments/assets/4224923a-3152-458d-bed6-433028d329b8)

```
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```
![image](https://github.com/user-attachments/assets/ce944399-7588-458d-a270-01ff6393c2d1)


```
sudo yum module list php
```
![image](https://github.com/user-attachments/assets/de51c7e8-3e37-435c-946d-d370fa334ef9)


```
sudo yum module reset php
```
![image](https://github.com/user-attachments/assets/0c97d292-1166-4c8d-9d53-b92912d36344)


```
sudo yum module enable php:remi-7.4
```
![image](https://github.com/user-attachments/assets/753b8743-188b-497c-a55f-c3884164c9e5)

```
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
```
![image](https://github.com/user-attachments/assets/6870fe91-413c-4506-b770-20390c391cb4)
![image](https://github.com/user-attachments/assets/c783b96f-29a7-41a3-8ffc-0a890de9f501)



```
sudo systemctl start php-fpm
```
![image](https://github.com/user-attachments/assets/ecf746f9-9142-42de-89df-c8240c49bb73)


```
sudo systemctl enable php-fpm
```
![image](https://github.com/user-attachments/assets/6909f311-5bff-485c-a924-e602291a51ca)


```
sudo setsebool -P httpd_execmem 1
```

![Uploading image.png…]()























