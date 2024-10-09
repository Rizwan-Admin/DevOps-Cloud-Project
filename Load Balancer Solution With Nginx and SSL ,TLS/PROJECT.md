# Load Balancer Solution With Nginx and SSL ,TLS

* Task
This project consists of two parts:

1. Configure Nginx as a Load Balancer
2. Register a new domain name and configure secured connection using SSL/TLS certificates
Your target architecture will look like this:

<img width="804" alt="image-42" src="https://github.com/user-attachments/assets/0f319c7f-04fb-4eec-87ee-e4d6918edfc2">

## Part 1 - Configure Nginx As A Load Balancer

1. Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 - this port is used for secured HTTPS connections)

2. Launch Ubuntu Server(Nginx_LB)
   


* Edit inbound rules

![image](https://github.com/user-attachments/assets/22c1c16e-6c26-4701-b682-8ea4e4fc68bb)

1. Open the /etc/hosts file on the machine that needs to resolve the web server names:
```
sudo nano /etc/hosts
```

![image](https://github.com/user-attachments/assets/d3cd90dc-4568-4789-9ca4-045c68f7551f)


3.Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

* Update the instance and Install Nginx Install Nginx
```
sudo apt update
```


```
sudo apt install nginx
```

```
sudo vi /etc/nginx/nginx.conf
```


```
http {
    # Other configurations
    
    upstream myproject {
        server Web1 weight=5;
        server Web2 weight=5;
    }

    server {
        listen 80;
        server_name www.domain.com;

        location / {
            proxy_pass http://myproject;
        }

        # Comment out the following line
        # include /etc/nginx/sites-enabled/*;
    }
}

```


* Restart Nginx and make sure the service is up and running


```
sudo systemctl restart nginx
sudo systemctl status nginx
```
![image](https://github.com/user-attachments/assets/a606aeb8-7fa8-448b-9931-33b44340e1b0)



Part 2 - Register a new domain name and configure secured connection using SSL/TLS certificates
1.  Register a new domain name with any registrar of your choice in any domain zone

cloudtimes.online


2. Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP


Elastic IP addresses:

![image](https://github.com/user-attachments/assets/cdb56af0-bf95-41b9-8d68-525466f570bf)
![image](https://github.com/user-attachments/assets/9fb59313-a9a7-4bd0-8f68-6e5857e9d315)

![image](https://github.com/user-attachments/assets/d7f715df-133c-4b40-acf7-edd75a95caa1)

Associate Ip
![image](https://github.com/user-attachments/assets/62d2ec2e-3235-4500-b3c7-8134a44fad68)


3. Update A record in your registrar to point to Nginx LB using Elastic IP address


![image](https://github.com/user-attachments/assets/4ec15567-7398-4aed-95ad-a780eff0c879)

4.Configure Nginx to recognize your new domain name

```
sudo vi /etc/nginx/nginx.conf
```
![image](https://github.com/user-attachments/assets/9ff8d1b2-caac-4ed0-85c7-47db0e2ffd2b)


Make sure snapd service is active and running


```
sudo systemctl status snapd

```
![image](https://github.com/user-attachments/assets/7cfeb394-365f-4e08-96cc-379c1492a1db)

* Install certbot
```
sudo snap install --classic certbot
```

* Request your certificate (just follow the certbot instructions - you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it on step 4).

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
```
sudo certbot --nginx
```

Request your certificate (just follow the certbot instructions - you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it on step 4).

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

```
sudo certbot --nginx
```
![image](https://github.com/user-attachments/assets/c40362fd-9606-416f-8dd2-52f9544f6b2f)












