# 💾 Disk & Filesystem Management in Linux

This section covers how to view, partition, format, mount, and maintain disks and filesystems in Linux. These skills are critical for any SysAdmin.

## 🔍 1. Viewing Disks & Partitions

| Command | Description | Example |
| :--- | :--- | :--- |
| `lsblk` | List block devices (disks and partitions) in a tree format | `lsblk` |
| `lsblk -f` | List block devices with **filesystem** and **UUID** info | `lsblk -f` |
| `fdisk -l` | List all partitions on all disks (legacy MBR) | `sudo fdisk -l` |
| `parted -l` | List all partitions (supports GPT) | `sudo parted -l` |
| `blkid` | Show **UUID** and filesystem type of partitions | `sudo blkid` |
| `df -h` | Show disk space usage in **human-readable** format | `df -h` |
| `df -hT` | Show disk space usage with **filesystem type** | `df -hT` |
| `du -sh /path` | Show the total size of a directory | `du -sh /home` |
| `du -sh /*` | Show sizes of top-level directories | `sudo du -sh /* 2>/dev/null` |

## 🛠️ 2. Partitioning Disks

### MBR (Master Boot Record)
- Maximum **4 primary** partitions.
- To create more than 4, use an **Extended** partition and create **Logical** partitions inside it.
- Maximum disk size: **2 TB**.

### GPT (GUID Partition Table)
- Maximum **128 partitions** (no need for Extended/Logical).
- Maximum disk size: **8 ZB**.
- Required for **UEFI** boot.

| Command | Description | Example |
| :--- | :--- | :--- |
| `fdisk /dev/sdb` | Open `fdisk` for MBR partitioning | `sudo fdisk /dev/sdb` |
| `gdisk /dev/sdb` | Open `gdisk` for GPT partitioning | `sudo gdisk /dev/sdb` |
| `parted /dev/sdb` | Open `parted` (supports both MBR and GPT) | `sudo parted /dev/sdb` |

**Key `fdisk` options:**
- `m`: Help
- `p`: Print partition table
- `n`: New partition
- `d`: Delete partition
- `t`: Change partition type
- `w`: Write changes and exit
- `q`: Quit without saving
- `o`: Create new MBR partition table
- `g`: Create new GPT partition table

## 📀 3. Creating Filesystems

| Command | Description | Example |
| :--- | :--- | :--- |
| `mkfs.ext4 /dev/sdb1` | Create **ext4** filesystem | `sudo mkfs.ext4 -F /dev/sdb1` |
| `mkfs.xfs /dev/sdb1` | Create **XFS** filesystem | `sudo mkfs.xfs -f /dev/sdb1` |
| `mkfs.vfat /dev/sdb1` | Create **FAT32** filesystem | `sudo mkfs.vfat /dev/sdb1` |
| `mkswap /dev/sdb2` | Create a **swap** partition | `sudo mkswap /dev/sdb2` |

**Note:** The `-F` (for ext4) and `-f` (for xfs) flags force the creation, ignoring any existing filesystem.

## 🔗 4. Mounting & Unmounting

| Command | Description | Example |
| :--- | :--- | :--- |
| `mount /dev/sdb1 /mnt` | Mount a partition to a directory | `sudo mount /dev/sdb1 /mnt` |
| `umount /mnt` | Unmount a partition | `sudo umount /mnt` |
| `mount -a` | Mount all filesystems listed in `/etc/fstab` | `sudo mount -a` |
| `mount -o ro /dev/sdb1 /mnt` | Mount as **read-only** | `sudo mount -o ro /dev/sdb1 /mnt` |

**The `/etc/fstab` file:**
Used to define permanent mounts at boot. Each line has 6 fields:
```
Example:
```
UUID=4e560f17-6965-46bf-8ac6-fbf1da5b6056 /mnt/p1 ext4 defaults 0 2
```
- **`<Dump>`**: `0` (no backup) or `1` (backup with `dump`).
- **`<Pass>`**: `0` (no fsck), `1` (first, for root), `2` (others).

## 🔍 5. Checking & Repairing Filesystems

| Command | Description | Example |
| :--- | :--- | :--- |
| `fsck /dev/sdb1` | Check and repair a filesystem | `sudo fsck /dev/sdb1` |
| `e2fsck /dev/sdb1` | Check/repair ext2/3/4 | `sudo e2fsck -fy /dev/sdb1` |
| `xfs_repair /dev/sdb1` | Check/repair XFS | `sudo xfs_repair /dev/sdb1` |

**Important:** You **must** unmount the filesystem before running `fsck` or `xfs_repair`.

## 🧠 6. Logical Volume Management (LVM)

LVM allows flexible resizing and pooling of disk space.

| Command | Description | Example |
| :--- | :--- | :--- |
| `pvcreate /dev/sdb1` | Create a **Physical Volume** | `sudo pvcreate /dev/sdb1` |
| `vgcreate vg0 /dev/sdb1` | Create a **Volume Group** | `sudo vgcreate vg0 /dev/sdb1` |
| `lvcreate -L 10G -n lv0 vg0` | Create a **Logical Volume** | `sudo lvcreate -L 10G -n lv0 vg0` |
| `mkfs.ext4 /dev/vg0/lv0` | Format the Logical Volume | `sudo mkfs.ext4 /dev/vg0/lv0` |
| `pvs` / `vgs` / `lvs` | Show LVM info | `sudo pvs` |

## 💡 7. Pro Tips

| Shortcut / Command | Description |
| :--- | :--- |
| `df -h` | Always your first step when disk is full |
| `du -sh * \| sort -rh \| head -n 10` | Find top 10 largest directories/files in the current folder |
| `mount \| column -t` | Show all current mounts in a clean table |
| `lsblk -f` | Check filesystem types and UUIDs quickly |
| `parted` | Use for GPT disks (modern systems) |

---
