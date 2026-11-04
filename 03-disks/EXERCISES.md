## 🧪 Disk & Filesystem - Exercises

This file contains practical exercises for disk partitioning, filesystem creation, mounting, and repair.

3 📝 Exercise 1: View Disks & Partitions

Task: Show all block devices, their filesystem types, and UUIDs.

Solution:

```bash
lsblk -f
sudo blkid
df -h
```

## 📝 Exercise 2: Create an MBR Partition Table with 5 Partitions

Task: Suppose you have a new disk /dev/sdb (2GB). Create an MBR partition table with 3 primary partitions (200MB each) and 1 extended partition containing 2 logical partitions (200MB each).

Step 1: Open fdisk:

```bash
sudo fdisk /dev/sdb
```

Step 2: Inside fdisk, press these keys one by one:

```text
o (create new MBR partition table)
n -> p -> 1 -> Enter -> +200M (primary partition 1)
n -> p -> 2 -> Enter -> +200M (primary partition 2)
n -> p -> 3 -> Enter -> +200M (primary partition 3)
n -> e -> 4 -> Enter -> Enter (extended partition using the rest)
n -> l -> Enter -> +200M (logical partition 1)
n -> l -> Enter -> Enter (logical partition 2 using the rest)
p (print partition table to verify)
w (write changes and exit)
```

Step 3: Verify partitions:

```bash
lsblk /dev/sdb
```

## 📝 Exercise 3: Create GPT Partition Table with XFS and ext4

Task: On a new disk /dev/sdc, create a GPT partition table with two partitions: one with ext4 (512MB) and one with XFS (512MB). Mount them to /mnt/gpt1 and /mnt/gpt2.

Step 1: Open parted:

```bash
sudo parted /dev/sdc
```

Step 2: Inside parted, type these commands one by one:

```text
(parted) mklabel gpt
(parted) mkpart primary ext4 1MiB 512MiB
(parted) mkpart primary xfs 512MiB 100%
(parted) print
(parted) quit
```

Step 3: Create filesystems:

```bash
sudo mkfs.ext4 -F /dev/sdc1
sudo mkfs.xfs -f /dev/sdc2
```

Step 4: Mount them:

```bash
sudo mkdir -p /mnt/gpt1 /mnt/gpt2
sudo mount /dev/sdc1 /mnt/gpt1
sudo mount /dev/sdc2 /mnt/gpt2
df -hT | grep sdc
```

## 📝 Exercise 4: Display File Metadata

Task: Show the metadata (inode, permissions, owner, size) of the file /etc/passwd.

Solution:

```bash
stat /etc/passwd
ls -li /etc/passwd
```

## 📝 Exercise 5: Label and Change UUID

Task: Label the filesystem /dev/sdc1 (ext4) as "MYDATA" and change its UUID to a random value.

Step 1: Unmount the filesystem:

```bash
sudo umount /dev/sdc1
```

Step 2: Set the label:

```bash
sudo e2label /dev/sdc1 MYDATA
```

Step 3: Change the UUID:

```bash
sudo tune2fs -U random /dev/sdc1
```

Step 4: Verify:

```bash
sudo blkid /dev/sdc1
```

## 📝 Exercise 6: Create Swap Space

Task: Create a 200MB swap partition on /dev/sdc3 and enable it permanently.

Step 1: Create the partition:

```bash
sudo fdisk /dev/sdc
```

Step 2: Inside fdisk, press these keys one by one:

```text
n -> 3 -> Enter -> +200M
t -> 3 -> 19 (set partition type to Linux swap)
w (write changes and exit)
```

Step 3: Create and enable swap:

```bash
sudo mkswap -L MYSWAP /dev/sdc3
sudo swapon /dev/sdc3
swapon --show
free -h
```

Step 4: Make it permanent (add to /etc/fstab):

```text
LABEL=MYSWAP none swap sw 0 0
```

## 📝 Exercise 7: Check and Repair Filesystem

Task: Check the filesystem /dev/sdc1 (ext4) and repair it if necessary.

Step 1: Unmount:

```bash
sudo umount /dev/sdc1
```

Step 2: Check and repair ext4:

```bash
sudo e2fsck -fy /dev/sdc1
```

Step 3: Check XFS (/dev/sdc2) without repairing:

```bash
sudo umount /dev/sdc2
sudo xfs_repair -n /dev/sdc2
```

## 📝 Exercise 8: Disk Usage with df and du

Task: Show disk usage of all filesystems, and find the 5 largest directories in /var.

Solution:

```bash
df -h
sudo du -sh /var/* 2>/dev/null | sort -rh | head -n 5
```

## 📝 Exercise 9: Create a 20MB File with Zeros (dd)

Task: Create a 20MB file named 20mb.img filled with zeros using dd.

Solution:

```bash
dd if=/dev/zero of=20mb.img bs=1M count=20
ls -lh 20mb.img
```
