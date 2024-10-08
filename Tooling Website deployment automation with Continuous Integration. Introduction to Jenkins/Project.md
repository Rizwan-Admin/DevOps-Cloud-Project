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
``
sudo systemctl status jenkins
```
![image](https://github.com/user-attachments/assets/c6530cac-906a-4285-b338-8cbf9f03bc00)


4. By default Jenkins server uses TCP port 8080- open it by creating a new Inbound Rule in your EC2 Security Group

![image](https://github.com/user-attachments/assets/0145054c-98de-428a-90e7-5989b83f0ca5)


5. Perform initial Jenkins setup.

* Note: From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

