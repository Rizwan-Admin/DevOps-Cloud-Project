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





