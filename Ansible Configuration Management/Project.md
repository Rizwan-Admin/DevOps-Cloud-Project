# Ansible Configuration Management 
# (Automate Project 7 to 10)

* You have been implementing some interesting projects up until now, and that is awesome.


In Projects 7 to 10 you had to perform a lot of manual operations to set up virtual servers, install and configure required software and deploy your web application.


This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated


with Ansible Configuration Management, at the same time you will become confident with writing code using declarative languages such as YAML.

# Let us get started!

# Ansible Client as a Jump Server (Bastion Host)
Jump Server = Bastion Host
* A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provides better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

# Tasks
Install and configure Ansible client to act as a Jump Server/Bastion Host
Create a simple Ansible playbook to automate servers configuration

# Step 1 - Install and Configure Ansible on EC2 Instance

1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.


2. In your GitHub account create a new repository and name it ansible-config-mgt.
3. Install Ansible (see: install Ansible with pip)

```
sudo apt update
```
```
sudo apt install ansible
```
Check your Ansible version by running ansible --version
```
ansible --version
```

![image](https://github.com/user-attachments/assets/6eb5c216-8b1b-467c-880d-71ea0b574805)


4. Configure Jenkins build job to archive your repository content every time you change it - this will solidify your Jenkins configuration skills acquired in Project 9

























































































