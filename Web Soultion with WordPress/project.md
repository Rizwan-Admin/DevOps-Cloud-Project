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

   Learn How to ADd EBS Voulme to an EC2 Instance https://www.youtube.com/watch?v=HPXnXkBzIHw

   
![image](https://github.com/user-attachments/assets/c45e449e-1e3d-4433-bffe-13ec03f36a99)

*    Create 3 volumes in the same AZ as web server EC2 , eacch of 10 GiB

![image](https://github.com/user-attachments/assets/3b65796b-acda-4968-8fb3-5aa5b1071352)

* Attach all three volumes one by one to your Web Server EC2 instance.
* use lsblk command to inspect what block are attached to the server

  ```
  lsblk
  ````
![image](https://github.com/user-attachments/assets/f4002bdc-66a5-468d-bc93-4163f10f65af)


* use df -h command to see all mount and free space on server
  ```
  df -h
  ```
  
![image](https://github.com/user-attachments/assets/90d1a877-d4a0-42f5-a5c7-dac61b30e98d)



* Use gdisk utility to create a single partition on each of the 3 disks
```
sudo gdisk /dev/xvdbf
```
![image](https://github.com/user-attachments/assets/2694dc96-00d2-402b-9208-f0ab9add179f)


* Use lsblk utility to view the newly configured partition on each of the 3 disks.
```
lsblk
```
![image](https://github.com/user-attachments/assets/18e19ffc-c58a-4ccc-9d2c-8045d8481a09)

* Install lvm2 package. Lvm2 is used for managing disk drives and other storage devices

```
sudo yum install lvm2
```
![image](https://github.com/user-attachments/assets/cb904063-2be1-47a3-b009-467f9fc04633)


* Run sudo lvmdiskscan to check for available partitions.
  
```
sudo lvmdiskscan
```
![image](https://github.com/user-attachments/assets/af63fa94-7cff-4550-aa83-743ee065caa3)

* Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM.
```
sudo pvcreate /dev/xvdbf1
sudo pvcreate /dev/xvdbg1
sudo pvcreate /dev/xvdbh1
```

![image](https://github.com/user-attachments/assets/5ae402d7-9975-43b4-816c-9327acabb61d)

* Verify that your Physical volume has been created successfully by running sudo pvs
  ```
  sudo pvs
  ```

![image](https://github.com/user-attachments/assets/70449c3c-511a-4da5-a979-cdc6f9497497)

* Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg

```
sudo vgcreate webdata-vg /dev/xvdbh1 /dev/xvdbg1 /dev/xvdbf1
```
![image](https://github.com/user-attachments/assets/07d7f8f8-6061-4b79-85a9-c8ea43c522cc)

* Verify that your VG has been created successfully by running sudo vgs

```
sudo vgs
```
![image](https://github.com/user-attachments/assets/810c5bbb-b7da-49e6-b7e9-a345aa3407d2)



* Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.


```
sudo lvcreate -n apps-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/ad6a2d09-ebc8-40df-85f8-73603cd4485b)

```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/804622c8-940c-49d0-966d-a3cd7dd51c03)


* Verify that your Logical Volume has been created successfully by running sudo lvs

```
sudo lvs
```
![image](https://github.com/user-attachments/assets/d944432b-42aa-4197-8c2c-7008edca7912)

* Verify the entire setup
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
```

![image](https://github.com/user-attachments/assets/d414372a-5a83-4ebc-8d60-501dcfe8670d)
```
sudo lsblk 
```
![image](https://github.com/user-attachments/assets/ece88432-2498-460e-b667-b395c9a37a78)

* Use mkfs.ext4 to format the logical volumes with ext4 filesystem
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```
![image](https://github.com/user-attachments/assets/2c8f8f97-ed7f-4f28-8bc5-2552a690cd28)

```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](https://github.com/user-attachments/assets/6431d36b-f7ef-4865-b285-f05a340667e7)

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
![image](https://github.com/user-attachments/assets/fbac7337-4bed-4fa2-8330-445d1877b008)



* Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)

```
sudo mount /dev/webdata-vg/logs-lv /var/log

```
* Restore log files back into /var/log directory
```
sudo rsync -av /home/recovery/logs/ /var/log
```
![image](https://github.com/user-attachments/assets/b02de7cf-7499-48ac-a01a-be17b0b29b33)



* Update /etc/fstab file so that the mount configuration will persist after restart of the server.
The UUID of the device will be used to update the /etc/fstab file

```
sudo blkid

```
![image](https://github.com/user-attachments/assets/154f144d-57db-4517-908d-54184900a600)

```
UUID=6c190cb5-9192-4e7a-a967-68b0f1f2a236
UUID=fc676e41-371a-4f87-bcc7-56f414b306a1
```


```
sudo vi /etc/fstab
```

![image](https://github.com/user-attachments/assets/259c503c-b238-49ea-b998-f37d5999e7b2)




 * Test the configuration and reload the daemon

```
sudo mount -a
```
![image](https://github.com/user-attachments/assets/36d50653-ed80-4238-ae50-c5195b0d45cb)




```
sudo systemctl daemon-reload
```

![image](https://github.com/user-attachments/assets/5eda33b5-112d-4677-8e8e-d663357adf1f)




* Verify your setup by running df -h, output must look like this:
```
df -h
```
![image](https://github.com/user-attachments/assets/eba402f7-0246-44b7-9833-e545feaa17b5)
















i::
