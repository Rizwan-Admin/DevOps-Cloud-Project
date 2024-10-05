
Here’s a step-by-step command guide for AWS Ubuntu or CentOS from basic to advanced concepts, focusing on system setup, troubleshooting, error handling, and monitoring:

### **Basic Commands: Getting Started with AWS Instances (Ubuntu/CentOS)**
1. **Update & Upgrade:**
   - Ubuntu:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```
   - CentOS:
     ```bash
     sudo yum update -y
     ```
   - Ensures the latest security patches and updates are installed.

2. **Installing Common Utilities:**
   ```bash
   sudo apt install curl wget net-tools -y  # Ubuntu
   sudo yum install curl wget net-tools -y  # CentOS
   ```

3. **Network Configuration:**
   - Check network interface:
     ```bash
     ifconfig   # Requires `net-tools` (Ubuntu/CentOS)
     ```
   - View network routes:
     ```bash
     route -n
     ```

4. **Creating SSH Keys:**
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

5. **Mounting EBS Volumes:**
   - Check attached block devices:
     ```bash
     lsblk
     ```
   - Format and mount a new EBS volume:
     ```bash
     sudo mkfs -t ext4 /dev/xvdf
     sudo mkdir /data
     sudo mount /dev/xvdf /data
     ```
   - Persist volume mount:
     ```bash
     echo "/dev/xvdf /data ext4 defaults,nofail 0 2" | sudo tee -a /etc/fstab
     ```

---

### **Intermediate Commands: Managing Services & Processes**

1. **Managing Services (e.g., Nginx/Apache):**
   - Start a service:
     ```bash
     sudo systemctl start nginx   # Ubuntu/CentOS
     ```
   - Enable on startup:
     ```bash
     sudo systemctl enable nginx
     ```
   - Check status:
     ```bash
     sudo systemctl status nginx
     ```

2. **Installing Web Servers:**
   - Ubuntu:
     ```bash
     sudo apt install nginx apache2 -y
     ```
   - CentOS:
     ```bash
     sudo yum install httpd -y
     ```

3. **Monitoring System Resources:**
   - CPU and memory usage:
     ```bash
     top
     htop  # Requires installation
     ```
   - Disk space:
     ```bash
     df -h
     ```

4. **Setting up Firewalls:**
   - Ubuntu:
     ```bash
     sudo ufw enable
     sudo ufw allow ssh
     sudo ufw allow 'Nginx Full'
     ```
   - CentOS:
     ```bash
     sudo firewall-cmd --permanent --add-service=http
     sudo firewall-cmd --reload
     ```

---

### **Advanced Commands: Error Resolution & Troubleshooting**

1. **Troubleshooting Network Issues:**
   - Ping a remote server:
     ```bash
     ping google.com
     ```
   - Check connectivity to MySQL Server on a specific port:
     ```bash
     nc -zv 35.154.194.242 3306
     ```

2. **Log Files Management:**
   - Viewing logs:
     ```bash
     tail -f /var/log/syslog   # Ubuntu
     tail -f /var/log/messages  # CentOS
     ```
   - Logs for Nginx or Apache:
     ```bash
     tail -f /var/log/nginx/error.log   # Nginx
     tail -f /var/log/apache2/error.log   # Apache (Ubuntu)
     ```

3. **Disk Space Troubleshooting:**
   - Find largest directories:
     ```bash
     du -sh /* | sort -rh | head -10
     ```

4. **Analyzing System Boot Issues:**
   - View boot logs:
     ```bash
     dmesg | less
     journalctl -b
     ```

---

### **Advanced Monitoring & Real-Time Tasks**

1. **Install Monitoring Tools:**
   - Ubuntu:
     ```bash
     sudo apt install sysstat iotop iftop -y
     ```
   - CentOS:
     ```bash
     sudo yum install sysstat iotop iftop -y
     ```

2. **Monitoring CPU, Disk, and Network:**
   - CPU and Disk Activity:
     ```bash
     iostat
     ```
   - Real-time network usage:
     ```bash
     iftop
     ```
   - Disk I/O:
     ```bash
     iotop
     ```

3. **Setting up Cron Jobs:**
   - Open crontab:
     ```bash
     crontab -e
     ```
   - Example job to run a backup script every day at midnight:
     ```bash
     0 0 * * * /home/user/backup.sh
     ```

4. **Monitoring MySQL Server Health:**
   - Checking MySQL status:
     ```bash
     mysqladmin -u root -p status
     ```
   - MySQL slow query log:
     ```bash
     SHOW VARIABLES LIKE 'slow_query_log';
     ```

---

### **Additional Tasks for Practice**

1. **Automating Backups with S3:**
   - Install AWS CLI:
     ```bash
     sudo apt install awscli   # Ubuntu
     sudo yum install awscli   # CentOS
     ```
   - Configure AWS CLI:
     ```bash
     aws configure
     ```
   - Create a backup script:
     ```bash
     #!/bin/bash
     tar -czvf /tmp/my-backup.tar.gz /data
     aws s3 cp /tmp/my-backup.tar.gz s3://your-bucket-name/
     ```

2. **Setting Up a LEMP Stack (Linux, Nginx, MySQL, PHP):**
   - Install PHP and Nginx:
     ```bash
     sudo apt install php-fpm php-mysql nginx -y   # Ubuntu
     sudo yum install php-fpm php-mysql nginx -y   # CentOS
     ```
   - Configure Nginx:
     ```bash
     sudo nano /etc/nginx/sites-available/default   # Ubuntu
     sudo nano /etc/nginx/conf.d/default.conf   # CentOS
     ```

---

These exercises span from beginner to advanced concepts, including system setup, troubleshooting, performance monitoring, and database management, suitable for real-world cloud engineering tasks on AWS.
