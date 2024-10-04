# **Partitioning Hard Drives in Linux: MBR and GPT Tutorial**

**Introduction:**

*Welcome to this tutorial on how to partition hard drives in Linux using two different partitioning schemes: MBR (Master Boot Record) and GPT (GUID Partition Table). This tutorial will guide you step-by-step through the process of partitioning using these schemes and explain the key differences between them.*

**Section 1: Understanding Partitioning Schemes**

### **1.1 What is MBR (Master Boot Record)?**

MBR is an older partitioning scheme that allows only up to 4 primary partitions per drive. If you need more partitions, you can create an extended partition, which lets you add logical partitions. Some key features include:

- **Max partitions:** 4 primary or 3 primary + 1 extended (for logical partitions)
- **Drive size limit:** Partitions must be less than 2 terabytes.
- **Partitioning tools:** Tools like `fdisk`, `cfdisk`, and `parted` can be used.
- **Compatibility:** MBR doesn’t work with modern UEFI systems but is still usable with older BIOS setups.

### **1.2 What is GPT (GUID Partition Table)?**

GPT is the modern standard for partitioning, offering more flexibility and reliability. It supports up to 128 partitions and allows much larger drives. Key features include:

- **Max partitions:** 128 per drive.
- **Drive size limit:** Up to 9.4 zettabytes!
- **Redundant partition tables:** Provides better reliability as it stores backup copies of partition tables.
- **UEFI support:** Required for modern UEFI motherboards.
- **Bootloader:** Typically uses `grub2` or `elilo`.

**Section 2: Setting Up Virtual Drives for Partitioning**

Before we begin partitioning, let’s set up our environment using VirtualBox.

### **2.1 Adding Virtual Hard Drives in VirtualBox**

- Open **VirtualBox** and select a virtual machine (we’ll use CentOS 7.5).
- Go to **Settings** > **Storage**.
- Under the **SATA controller**, click the disk icon to add a new hard disk.
- Choose to create a new disk of **20 GB** size.
- Repeat the steps to create a second disk for a different partitioning scheme.

Now, you’re ready to start partitioning!

**Section 3: Partitioning with MBR Using Fdisk**

### **3.1 MBR Partitioning Steps**

- Boot up your virtual machine and log in.
- Open a terminal and run the following command to list all available disks:

    ```bash
    sudo fdisk -l
    ```

- Identify the new drive (likely `/dev/sdb`).
- Start partitioning using `fdisk`:

    ```bash
    sudo fdisk /dev/sdb
    ```

- You will enter the `fdisk` interface. Use the following steps to create partitions:

    1. Press `m` to bring up the menu.
    2. Press `n` to create a new partition.
    3. Choose `p` for primary partition.
    4. Set the partition number (default is 1).
    5. Set the first sector (default is the beginning of the drive).
    6. Choose a size for the partition (e.g., `+5G` for 5 GB).
    7. Repeat the process for more partitions, or press `w` to write the changes and exit.

- If you need to delete a partition, press `d` and follow the prompts.

### **3.2 Using Extended and Logical Partitions**

MBR allows only 4 primary partitions. If you need more:

- Create an **extended partition** by selecting `e` during partition creation.
- Then, create **logical partitions** inside the extended partition by selecting `l` during partition creation.

**Section 4: Partitioning with GPT**

GPT allows you to have more partitions and supports larger disk sizes.

### **4.1 GPT Partitioning with Parted**

To create partitions using GPT, follow these steps:

- Start the partitioning tool `parted`:

    ```bash
    sudo parted /dev/sdc
    ```

- Set the partitioning scheme to GPT:

    ```bash
    mklabel gpt
    ```

- Create partitions:

    ```bash
    mkpart primary ext4 1MiB 5GiB
    ```

- Repeat the process for more partitions, or type `print` to view the partition table.

- After partitioning, quit the tool:

    ```bash
    quit
    ```

**Section 5: Formatting Partitions**

Once partitions are created, they need to be formatted:

- To format a partition to `ext4`:

    ```bash
    sudo mkfs.ext4 /dev/sdb1
    ```

- For swap partitions, use:

    ```bash
    sudo mkswap /dev/sdb1
    ```

**Section 6: Verifying Partitions**

To check the partitions you’ve created, use `fdisk`:

```bash
sudo fdisk -l
```

This will display all the partitions and their sizes, types, and mount points.

**Conclusion:**

*In this tutorial, you learned the basics of partitioning hard drives in Linux using both MBR and GPT. MBR is suitable for older systems, while GPT is the preferred method for modern systems due to its support for larger partitions and better reliability. Now, you should be ready to start partitioning drives for your Linux systems using the methods shown.*

---

