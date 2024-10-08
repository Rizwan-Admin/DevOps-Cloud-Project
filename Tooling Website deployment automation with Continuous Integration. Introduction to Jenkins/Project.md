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

![image](https://github.com/user-attachments/assets/9ef80627-b12d-47d6-a276-b14f8b98f000)

![image](https://github.com/user-attachments/assets/edd91028-3db1-4671-89f2-f940d85068c7)


![image](https://github.com/user-attachments/assets/cdb551b9-6afd-4b63-b78b-390ce75bcad4)




2. Go to Jenkins web console, click "New Item" and create a "Freestyle project"



![image](https://github.com/user-attachments/assets/c82f4781-901f-4f0e-a8fa-16f720eb48e8)




![image](https://github.com/user-attachments/assets/31c9a7d4-c467-4318-8ee5-27992fea84ff)



To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself

![image](https://github.com/user-attachments/assets/4a6a1a4d-21fc-4413-8bed-3af8777d01be)


* Save the configuration and let us try to run the build. For now we can only do it manually. Click "Build Now" button, if you have configured everything correctly, the build will be successfull and you will see it under #1




![image](https://github.com/user-attachments/assets/2d7e3e49-b29a-484d-81a6-415ea0686b5a)

* You can open the build and check in "Console Output" if it has run successfully.


* If so - congratulations! You have just made your very first Jenkins build!


* But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.


3. Click "Configure" your job/project and add these two configurations

* Configure triggering the job from GitHub webhook:

![image](https://github.com/user-attachments/assets/3a0c8315-a03f-48b2-8051-3029fff13a74)




* Configure "Post-build Actions" to archive all the files - files resulted from a build are called "artifacts".


![image](https://github.com/user-attachments/assets/71b55987-4e13-4244-b13c-dc76b0cd9d48)

* Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.


* You will see that a new build has been launched automatically (by webhook) and you can see its results - artifacts, saved on Jenkins server.

![image](https://github.com/user-attachments/assets/d72c90c9-d16e-4e7e-a2fe-a3df8b6212fe)

![image](https://github.com/user-attachments/assets/7e4f3115-17d4-4476-8cbc-286c034e0deb)


You have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as 'push' because the changes are being 'pushed' and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstreadm) from another (upstream), poll GitHub periodically and others.


*  By default, the artifacts are stored on Jenkins server locally
```
ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
```
![image](https://github.com/user-attachments/assets/67255df2-85ca-4889-b545-9619c90be377)




Step 3 - Configure Jenkins to copy files to NFS server via SSH

* Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.
* Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called "Publish Over SSH".

  1. Install "Publish Over SSH" plugin.
* On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item.
* On "Available" tab search for "Publish Over SSH" plugin and install it
![image](https://github.com/user-attachments/assets/50b53c8c-3a58-4942-bafb-8455c34243a4)


2.Configure the job/project to copy artifacts over to NFS server.


* On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

![image](https://github.com/user-attachments/assets/d168c56a-4006-4fd2-92dd-ad5dbf06badd)

* Save the configuration, open your Jenkins job/project configuration page and add another one "Post-build Action"


![image](https://github.com/user-attachments/assets/176c340d-9e9e-4ed8-8a13-34f4d129813b)
![image](https://github.com/user-attachments/assets/a3403774-1659-496d-8abb-3b180523f066)


* Save this configuration and go ahead, change something in README.MD file in your GitHub Tooling repository.

* ### If you find an error, change security settings of NFS Server
```
sudo chown -R nobody:nobody /mnt
sudo chmod -R 777 /mnt

```

![image](https://github.com/user-attachments/assets/5061195d-22dd-4f6b-bb55-2679c5501c9f)


* To make sure that the files in /mnt/apps have been udated - connect via SSH/Putty to your NFS server and check README.MD file
```
cat /mnt/apps/README.md
```

![image](https://github.com/user-attachments/assets/4dc161f1-7d85-44ec-a39f-c907646ed618)


* If you see the changes you had previously made in your GitHub - the job works as expected.

* ## Conclusion!
* Implemented Continous Integration solution using Jenkins CI. Watch out for advanced CI configurations in upcoming projects.
* This project highlights the impact of automation on software delivery and sets a strong foundation for continued advancements in the development and deployment workflow.
























