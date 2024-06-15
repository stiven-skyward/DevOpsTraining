# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 1 - Prepare a Web Server

### Launch an EC2 Instance

Launch an EC2 instance that will serve as your "Web Server". Create 3 volumes in the same Availability Zone (AZ) as your Web Server EC2 instance, each with a size of 10 GiB.

### Attach Volumes to the EC2 Instance

Attach all three volumes one by one to your Web Server EC2 instance.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/d46e8d17-8a45-4632-97ac-dbd4c6c40511)

### Open the Linux Terminal

Open up the Linux terminal to begin configuration.

### Inspect Block Devices

Use the `lsblk` command to inspect what block devices are attached to the server. Notice the names of your newly created devices. All devices in Linux reside in the `/dev/` directory. Inspect it with `ls /dev/` and make sure you see all 3 newly created block devices there - their names will likely be `xvdf`, `xvdh`, and `xvdg`.

```sh
lsblk
ls /dev/
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/ffbfbcf7-93c2-4784-83d0-12c06eb9ee7d)

### View Mounts and Free Space

Use the `df -h` command to see all mounts and free space on your server.

```sh
df -h
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/702a5c29-d455-4693-a765-1d2fd3b2db3b)

### Create Partitions on Each Disk

Use the `gdisk` utility to create a single partition on each of the 3 disks:

```sh
sudo gdisk /dev/xvdb
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

Repeat the above process for `/dev/xvdc` and `/dev/xvdd`.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/89ebad08-1919-4bc1-b547-3a555a5673e1)

### View Configured Partitions

Use the `lsblk` utility to view the newly configured partition on each of the 3 disks.

```sh
lsblk
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/a91726b6-7207-46f4-9e45-f792701bd58a)

### Install `lvm2` Package

Install the `lvm2` package and check for available partitions.

```sh
sudo yum install lvm2
sudo lvmdiskscan
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/e59e922a-9e89-427c-bcf9-48dad4292b2c)

### Mark Disks as Physical Volumes (PVs)

Use the `pvcreate` utility to mark each of the 3 disks as physical volumes (PVs) to be used by LVM.

```sh
sudo pvcreate /dev/xvdb1
sudo pvcreate /dev/xvdc1
sudo pvcreate /dev/xvdd1
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/946226e6-4d92-4d00-9c0f-5f391cebb46a)

Verify that your Physical Volumes have been created successfully by running:

```sh
sudo pvs
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/c398d0a1-32bb-4d4d-8f67-9e77453f7e87)

### Create a Volume Group (VG)

Use the `vgcreate` utility to add all 3 PVs to a volume group (VG). Name the VG `webdata-vg`.

```sh
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/93a222f7-83f1-430f-8664-c3e790b596b2)

Verify that your VG has been created successfully by running:

```sh
sudo vgs
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/d84cc4fa-ebba-4614-98ed-b0dea490eebb)

### Create Logical Volumes (LVs)

Use the `lvcreate` utility to create 2 logical volumes: `apps-lv` (use half of the PV size) and `logs-lv` (use the remaining space).

```sh
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/50f734af-9f03-43fc-a20d-b17a866f7bcd)

Verify that your Logical Volumes have been created successfully by running:

```sh
sudo lvs
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/5f0ca39c-21f9-4550-bd20-2edbef0d6aa5)

### Verify the Entire Setup

```sh
sudo vgdisplay -v
sudo lsblk
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/639f1639-0224-4407-b64e-18d7edc81266)

### Format the Logical Volumes

Use `mkfs.ext4` to format the logical volumes with the ext4 filesystem.

```sh
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/c02d5f58-316f-41b9-a4cc-3873c5eecc67)

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
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/66e27659-b4b4-4870-b400-a3bb94331790)

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
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/0aaa7376-f88c-4573-be6b-180914dfee2b)

### Update `/etc/fstab` for Persistent Mounts

Get the UUID of the devices.

```sh
sudo blkid
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/7bb73091-ed17-4a98-b47f-0cdb07278366)

Update `/etc/fstab` with the UUIDs. Open the file with:

```sh
sudo nano /etc/fstab
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
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/ebcbc15e-0864-4335-91a9-030ddc1d267b)
