
# Prerequisites
Make sure that you have the following servers installed and configured within the previous project:

-Two RHEL8 Web Servers
-One MySQL DB Server (based on Ubuntu 20.04)
-One RHEL8 NFS server


* Configure Apache As A Load Balancer
1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:
2. Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.
3. Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:



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

#Restart apache2 service
sudo systemctl restart apache2
![image](https://github.com/user-attachments/assets/421c021b-f2a9-4840-8b87-896d72001897)

![image](https://github.com/user-attachments/assets/e8a54748-9afb-4366-8729-f08b9004c02e)







![image](https://github.com/user-attachments/assets/c75d8b7b-ed70-4c4b-af57-b1c00cddb3a2)


![image](https://github.com/user-attachments/assets/933af28a-8202-4627-9df1-045d73ca6aea)


![image](https://github.com/user-attachments/assets/18482658-120c-433f-bdf1-b258b00a638f)


![Uploading image.png…]()






















