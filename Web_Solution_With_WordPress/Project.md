
## Step 1 — Prepare a Web Server

* Launch ec2 --> Web-Server
  ![image](https://github.com/user-attachments/assets/f7697b1e-7b77-4536-b3f1-4da5f6432335)

  *ADD EBS volume to an ec2
  ![image](https://github.com/user-attachments/assets/b8df04e8-2587-40a3-a0b0-4ff5993cac3b)

connect via ssh  & run comand lsblk

```
lsblk
```

![image](https://github.com/user-attachments/assets/f6de1242-7b48-430f-898b-60d6befde2a8)



Use df -h command to see all mounts and free space on your serve


```
df -h
```
![image](https://github.com/user-attachments/assets/f448ab6c-5ac1-48be-a8e1-21fc550772d7)

* Use gdisk utility to create a single partition on each of the 3 disks
```
sudo gdisk /dev/xvdf

```

![image](https://github.com/user-attachments/assets/37f74747-9d73-49bf-abb5-3b978c8a0b24)

```
lsblk
```

![image](https://github.com/user-attachments/assets/da440940-7163-4dba-b070-7690300e563b)


Install lvm2 package using sudo yum install lvm2. Run sudo lvmdiskscan command to check for available partitions.
```
sudo yum iynstall lvm2
```
![image](https://github.com/user-attachments/assets/be74f249-2b30-49b6-ac78-95e9a366c2f7)



```
sudo lvmdiskscan
```

![image](https://github.com/user-attachments/assets/8d2c6fb7-f5cb-4a54-85bd-5e5d2b604df5)




* Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

```
sudo pvcreate /dev/xvdf1
```
![image](https://github.com/user-attachments/assets/4d103bfe-4f67-4ef6-96d1-af84c0b0a26e)

```
sudo pvcreate /dev/xvdbg1
```
```
sudo pvcreate /dev/xvdbh1
```
![image](https://github.com/user-attachments/assets/7d6b8d4a-0d92-4307-b4a0-84e4b74b61cb)


* Verify that your Physical volume has been created successfully by running 

```
sudo pvs
```
![image](https://github.com/user-attachments/assets/7bddf6b4-0d62-408d-a62c-197b8b9aff37)



* Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
```
sudo vgcreate webdata-vg /dev/xvdbh1 /dev/xvdbg1 /dev/xvdbf1
```

![image](https://github.com/user-attachments/assets/7cdd46e9-4223-42d1-aba3-b2603a2771f9)




Verify that your VG has been created successfully by running

```
sudo vgs
```
![image](https://github.com/user-attachments/assets/8b3e6231-a762-46ac-9f54-f29776850ceb)


* Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.


```
sudo lvcreate -n apps-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/d205d3cf-56d8-476f-aab6-25efd055ea59)

```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/f325ca3c-b657-4451-824a-8844dca9e369)

* Verify that your Logical Volume has been created successfully by running
  ```
  sudo lvs
  ```
  
![image](https://github.com/user-attachments/assets/21f801d5-02a6-4474-8989-12768f3b6d58)


Verify the entire setup
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
```
![image](https://github.com/user-attachments/assets/01f74f33-d67d-424d-ab68-fff71424ba58)


```
sudo lsblk
```
![image](https://github.com/user-attachments/assets/062d0149-58f9-45ff-ad91-18af53892308)


* Use mkfs.ext4 to format the logical volumes with ext4 filesystem
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```
![image](https://github.com/user-attachments/assets/805c2e90-6345-4dad-b2ab-775ee97e6d9a)

```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](https://github.com/user-attachments/assets/222c6761-6adf-4701-b152-326bb6b6ef9f)


* Create /var/www/html directory to store website files
  ```
  sudo mkdir -p /var/www/html
  ```
* Create /home/recovery/logs to store backup of log data
  ```
  sudo mkdir -p /home/recovery/logs
  ```
* Mount /var/www/html on apps-lv logical volume

```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```
![image](https://github.com/user-attachments/assets/3b1cef8b-115b-4949-b82a-2090f7d2d619)



* Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)

```
sudo rsync -av /var/log/ /home/recovery/logs/
```

![image](https://github.com/user-attachments/assets/08c39182-1505-4763-9a32-e2aae0eb7230)


* Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)

```
sudo mount /dev/webdata-vg/logs-lv /var/log
```


* Restore log files back into /var/log directory

```
sudo rsync -av /home/recovery/logs/ /var/log

```

![image](https://github.com/user-attachments/assets/70452252-39be-4f95-8c01-0dd0662cbb5c)




* Update /etc/fstab file so that the mount configuration will persist after restart of the server.
The UUID of the device will be used to update the /etc/fstab file;
```
sudo blkid
```

UUID=d9b08fef-59cd-4788-b429-a071b1942fe9

UUID=9d89b04e-c0bf-4b1f-80ce-964ad2b7ddc5
:
UUID=d9b08fef-59cd-4788-b429-a071b1942fe9 /var/www/html ext4 defaults 0i 0

UUID=9d89b04e-c0bf-4b1f-80ce-964ad2b7ddc5 /var/log ext4 defaults 0 0

```
sudo vi /etc/fstab
```


Test the configuration and reload the daemon
```

sudo mount -a
```
![image](https://github.com/user-attachments/assets/9cd2a0f0-a30a-4ee8-8859-7efc73122695)


```
sudo systemctl daemon-reload

```

* Verify your setup by running df -h, output must look like this:
```
df -f
```

![image](https://github.com/user-attachments/assets/02269fcd-3423-43f7-a539-7fce67eb3c66)














