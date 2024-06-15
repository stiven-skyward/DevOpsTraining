# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 1 - Prepare a Web Server

### Launch an EC2 Instance

Launch an EC2 instance that will serve as your "Web Server". Create 3 volumes in the same Availability Zone (AZ) as your Web Server EC2 instance, each with a size of 10 GiB.

### Attach Volumes to the EC2 Instance

Attach all three volumes one by one to your Web Server EC2 instance.

### Open the Linux Terminal

Open up the Linux terminal to begin configuration.

### Inspect Block Devices

Use the `lsblk` command to inspect what block devices are attached to the server. Notice the names of your newly created devices. All devices in Linux reside in the `/dev/` directory. Inspect it with `ls /dev/` and make sure you see all 3 newly created block devices there - their names will likely be `xvdf`, `xvdh`, and `xvdg`.

```sh
lsblk
ls /dev/
```

### View Mounts and Free Space

Use the `df -h` command to see all mounts and free space on your server.

```sh
df -h
```

### Create Partitions on Each Disk

Use the `gdisk` utility to create a single partition on each of the 3 disks:

```sh
sudo gdisk /dev/xvdf
```

#### Example of `gdisk` Usage:

```plaintext
GPT fdisk (gdisk) version 1.0.3

Partition table scan:
  MBR: not present
  BSD: not present
  APM: not present
  GPT: not present

Creating new GPT entries.

Command (? for help): n
Partition number (1-128, default 1): 1
First sector (34-20971486, default = 2048) or {+-}size{KMGTP}: <Enter>
Last sector (2048-20971486, default = 20971486) or {+-}size{KMGTP}: <Enter>
Current type (L): 8300
Hex code or GUID (L to show codes, Enter = 8300): 8E00
Changed type of partition to 'Linux LVM'

Command (? for help): w
Do you want to proceed? (Y/N): yes
```

Repeat the above process for `/dev/xvdg` and `/dev/xvdh`.

### View Configured Partitions

Use the `lsblk` utility to view the newly configured partition on each of the 3 disks.

```sh
lsblk
```

### Install `lvm2` Package

Install the `lvm2` package and check for available partitions.

```sh
sudo yum install lvm2
sudo lvmdiskscan
```

### Mark Disks as Physical Volumes (PVs)

Use the `pvcreate` utility to mark each of the 3 disks as physical volumes (PVs) to be used by LVM.

```sh
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

Verify that your Physical Volumes have been created successfully by running:

```sh
sudo pvs
```

### Create a Volume Group (VG)

Use the `vgcreate` utility to add all 3 PVs to a volume group (VG). Name the VG `webdata-vg`.

```sh
sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```

Verify that your VG has been created successfully by running:

```sh
sudo vgs
```

### Create Logical Volumes (LVs)

Use the `lvcreate` utility to create 2 logical volumes: `apps-lv` (use half of the PV size) and `logs-lv` (use the remaining space).

```sh
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

Verify that your Logical Volumes have been created successfully by running:

```sh
sudo lvs
```

### Verify the Entire Setup

```sh
sudo vgdisplay -v
sudo lsblk
```

### Format the Logical Volumes

Use `mkfs.ext4` to format the logical volumes with the ext4 filesystem.

```sh
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

### Create Directories for Mount Points

Create the directories to store website files and backup log data.

```sh
sudo mkdir -p /var/www/html
sudo mkdir -p /home/recovery/logs
```

### Mount Logical Volumes

Mount `/var/www/html` on `apps-lv` logical volume.

```sh
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

### Backup Log Files

Use the `rsync` utility to backup all the files in the log directory `/var/log` into `/home/recovery/logs`.

```sh
sudo rsync -av /var/log/ /home/recovery/logs/
```

### Mount `logs-lv` Logical Volume

Mount `/var/log` on `logs-lv` logical volume.

```sh
sudo mount /dev/webdata-vg/logs-lv /var/log
```

### Restore Log Files

Restore log files back into the `/var/log` directory.

```sh
sudo rsync -av /home/recovery/logs/ /var/log
```

### Update `/etc/fstab` for Persistent Mounts

Get the UUID of the devices.

```sh
sudo blkid
```

Update `/etc/fstab` with the UUIDs. Open the file with:

```sh
sudo vi /etc/fstab
```

Add the following lines (replace with your own UUIDs):

```plaintext
UUID=<UUID-for-apps-lv> /var/www/html ext4 defaults 0 0
UUID=<UUID-for-logs-lv> /var/log ext4 defaults 0 0
```

### Test the Configuration

```sh
sudo mount -a
sudo systemctl daemon-reload
```

Verify your setup by running:

```sh
df -h
```