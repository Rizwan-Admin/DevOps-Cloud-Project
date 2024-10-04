To create and manage Linux partitions using the `fdisk` command, here's a guide that covers various aspects, from basic partitioning to advanced troubleshooting and monitoring. This guide is tailored for AWS Ubuntu or CentOS environments.

### **Prerequisites**
- Basic knowledge of Linux command line
- Access to an AWS Ubuntu or CentOS instance
- Root or `sudo` privileges

---

### **Step 1: Checking Available Disks**
Before creating partitions, check available disks on the system:

```bash
sudo fdisk -l
```

This lists all disks and their partitions. Take note of the disk you'd like to partition (e.g., `/dev/xvdb`).

---

### **Step 2: Creating a New Partition**

1. Start `fdisk` on the chosen disk:
   ```bash
   sudo fdisk /dev/xvdb
   ```
2. Enter interactive mode:
   - Type `n` for a new partition.
   - Choose the partition type (Primary or Extended):
     - Type `p` for Primary (up to 4 primary partitions).
   - Select a partition number (1-4).
   - Specify the first and last sectors or accept defaults for full size.
   
3. Save the partition table changes:
   - Type `w` to write the changes and exit.

---

### **Step 3: Formatting the Partition**
Once the partition is created, format it with a filesystem like `ext4`:

```bash
sudo mkfs.ext4 /dev/xvdb1
```

Replace `/dev/xvdb1` with the newly created partition.

---

### **Step 4: Mounting the Partition**
Mount the new partition to a directory:

```bash
sudo mkdir /mnt/mydata
sudo mount /dev/xvdb1 /mnt/mydata
```

To make the partition mount automatically on boot, edit the `/etc/fstab` file:
```bash
sudo nano /etc/fstab
```

Add the following line:
```bash
/dev/xvdb1 /mnt/mydata ext4 defaults 0 0
```

---

### **Step 5: Partition Troubleshooting and Monitoring**
1. **Troubleshooting Mounting Errors**
   - Check the mount status with:
     ```bash
     df -h
     ```
   - If you face mounting issues, check logs:
     ```bash
     dmesg | grep mount
     ```
   
   Errors like "wrong fs type" indicate incorrect filesystem formatting.

2. **Resizing a Partition**
   - You can resize a partition using `resize2fs` after unmounting it:
     ```bash
     sudo umount /mnt/mydata
     sudo resize2fs /dev/xvdb1
     sudo mount /dev/xvdb1 /mnt/mydata
     ```

3. **Checking Partition Health**
   - Use `fsck` to check for filesystem errors:
     ```bash
     sudo fsck /dev/xvdb1
     ```

---

### **Step 6: Real-Time Partition Monitoring**

1. **Monitoring Disk Usage**
   Monitor disk usage in real-time using:
   ```bash
   watch -n 2 df -h
   ```

2. **Disk I/O Monitoring**
   Use `iostat` (from `sysstat` package) to monitor disk input/output:
   ```bash
   sudo apt-get install sysstat   # For Ubuntu
   sudo yum install sysstat       # For CentOS
   iostat -x 5
   ```

---

### **Step 7: Advanced Partition Management**

1. **Creating Swap Partition**
   - Create a new partition for swap:
     ```bash
     sudo fdisk /dev/xvdb
     ```
   - Type `t` to change the partition type.
   - Type `82` for Linux Swap.
   - Format the partition as swap:
     ```bash
     sudo mkswap /dev/xvdb2
     sudo swapon /dev/xvdb2
     ```
   - Add it to `/etc/fstab` for persistence:
     ```bash
     /dev/xvdb2 none swap sw 0 0
     ```

2. **Monitoring Swap Usage**
   ```bash
   free -h
   ```

---

### **Step 8: Error Resolution Tips**

1. **Fixing Partition Table Errors**
   - If you encounter "Invalid partition table" errors:
     ```bash
     sudo partprobe /dev/xvdb
     ```
   - This reloads the partition table without requiring a reboot.

2. **Recovering Data from Corrupted Partitions**
   - Use `testdisk` for recovering data from corrupted partitions:
     ```bash
     sudo apt-get install testdisk
     sudo testdisk /dev/xvdb
     ```

---

### **Step 9: Best Practices**
- Always back up important data before making changes to partitions.
- Regularly monitor disk usage and performance using `iostat` and `df -h`.
- Ensure that `/etc/fstab` entries are correct to prevent boot issues.

---

### **Step 10: Practical Exercises**

1. **Exercise 1: Create Multiple Partitions**
   - Create two primary partitions and one extended partition with two logical partitions.

2. **Exercise 2: Resize a Partition**
   - Create a partition, fill it with data, and then resize it.

3. **Exercise 3: Swap Management**
   - Create a swap partition, monitor swap usage, and then remove the swap partition.

4. **Exercise 4: Real-Time Monitoring**
   - Monitor disk I/O during a file transfer and analyze the performance using `iostat`.

By practicing these commands and tasks on AWS Ubuntu or CentOS, you'll build your competency from basic to advanced concepts.
