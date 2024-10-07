
# Prerequisites
Make sure that you have the following servers installed and configured within the previous project:

-Two RHEL8 Web Servers
-One MySQL DB Server (based on Ubuntu 20.04)
-One RHEL8 NFS server
![image](https://github.com/user-attachments/assets/e8a54748-9afb-4366-8729-f08b9004c02e)


* Configure Apache As A Load Balancer
1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:
2. Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.
3. Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:
![image](https://github.com/user-attachments/assets/421c021b-f2a9-4840-8b87-896d72001897)



#Install apache2
```
sudo apt update
```

```
sudo apt install apache2 -y
```
```
sudo apt-get install libxml2-dev
```


#Enable following modules:
```
sudo a2enmod rewrite
```
```
sudo a2enmod proxy
```
```
sudo a2enmod proxy_balancer
```
```
sudo a2enmod proxy_http
```
```
sudo a2enmod headers
```
```
sudo a2enmod lbmethod_bytraffic
```
![image](https://github.com/user-attachments/assets/9bd36638-a38d-4e05-a255-ebc65118e46a)


#Restart apache2 service
```
sudo systemctl restart apache2
```
* Make sure apache2 is up and running

```
sudo systemctl status apache2
```
![image](https://github.com/user-attachments/assets/afa29e5b-2970-4f47-b095-7d6f744a8d14)

* ### Configure load balancing
```
sudo vi /etc/apache2/sites-available/000-default.conf
```


```powershell
#Add this configuration into this section <VirtualHost *:80>  </VirtualHost>

<Proxy "balancer://mycluster">
               BalancerMember http://172.31.0.191:80 loadfactor=5 timeout=1
               BalancerMember http://172.31.2.108 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
```
![image](https://github.com/user-attachments/assets/c62eb0b4-a4e5-4c44-9743-b7a061b0a5f5)

#Restart apache server

```
sudo systemctl restart apache2

```
![image](https://github.com/user-attachments/assets/a67948b0-0653-4f01-9ee3-01101294fd07)



4. Verify that our configuration works - try to access your LB's public IP address or Public DNS name from your browser:

```
http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php

```
```
http://35.154.65.190/login.php
```
![image](https://github.com/user-attachments/assets/31bbe122-af7f-4bd6-b7b8-0ff2cea32c78)
### Done!
### Main Page
![image](https://github.com/user-attachments/assets/ff4acd57-646d-41d5-92dd-aa8960914622)

![image](https://github.com/user-attachments/assets/554d6122-2b40-4721-bee0-43f46877188a)



* Note: If in the previous project, you mounted /var/log/httpd/ from your Web Servers to the NFS server - unmount them and make sure that each Web Server has its own log directory.
```
sudo tail -f /var/log/httpd/access_log
```
* ### WebServer1 Logs
![image](https://github.com/user-attachments/assets/638b5b4f-0e31-47c9-b45d-b5d3dbe3fbef)

* ### WebServer2 Logs
  
![image](https://github.com/user-attachments/assets/03778743-39ac-48ae-af57-7fbfc9ad9aca)


![image](https://github.com/user-attachments/assets/c75d8b7b-ed70-4c4b-af57-b1c00cddb3a2)


![image](https://github.com/user-attachments/assets/933af28a-8202-4627-9df1-045d73ca6aea)


![image](https://github.com/user-attachments/assets/18482658-120c-433f-bdf1-b258b00a638f)


![Uploading image.png…]()






















