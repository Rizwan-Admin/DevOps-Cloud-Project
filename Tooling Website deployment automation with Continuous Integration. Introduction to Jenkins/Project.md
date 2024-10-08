# Tooling Website deployment automation with Continuous Integration. (Jenkins)

### Task
Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

![image](https://github.com/user-attachments/assets/2bb7aaec-37d5-49aa-a9f8-f1b63f4c0fd0)

2. Install JDK (since Jenkins is a Java-based application)
```
sudo apt update
```
```
sudo apt install default-jdk-headless
```
![image](https://github.com/user-attachments/assets/dc21e9b2-2b4e-4931-aacf-fa86a0c56a95)


3. Install Jenkins

```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

Make sure Jenkins is up and running
```
sudo systemctl status jenkins
```
![image](https://github.com/user-attachments/assets/c6530cac-906a-4285-b338-8cbf9f03bc00)


4. By default Jenkins server uses TCP port 8080- open it by creating a new Inbound Rule in your EC2 Security Group

![image](https://github.com/user-attachments/assets/0145054c-98de-428a-90e7-5989b83f0ca5)


5. Perform initial Jenkins setup.

* Note: From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
```
3.110.183.142:8080
```
![image](https://github.com/user-attachments/assets/eae33b80-e85e-46da-ab99-76cd4d45226b)

* Retrieve it from your server:

```powershell
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![image](https://github.com/user-attachments/assets/d947ae9f-7de6-49a6-8829-7edf26752e28)

```
1f6206df1c4640d796477dd0c937451e
```

![image](https://github.com/user-attachments/assets/1627d0d8-4cd3-493e-8076-41e12e341420)


* Once plugins installation is done – create an admin user and you will get your Jenkins server address.
![image](https://github.com/user-attachments/assets/dc131bd7-8ff6-4722-a8b2-a3aa228815b3)


* The installation is completed!

![image](https://github.com/user-attachments/assets/11c54415-8ab9-46d7-93ac-a5efc3b1077c)

![image](https://github.com/user-attachments/assets/028bf83e-bea3-423d-9d92-25bb94a52f9c)

![image](https://github.com/user-attachments/assets/7b51bab4-f018-4444-b7fa-e22d6c3ea89a)
![image](https://github.com/user-attachments/assets/f98ea42f-e8e8-46ee-8fda-d809c8884b98)


### Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks

* In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub webhooks and will execute a 'build' task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings







