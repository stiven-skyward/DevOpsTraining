# DevOpsTraining
**DevOps/Cloud Training Material**

# NFS Server Configuration on RHEL 9 for Web Servers and Jenkins

## Step 1: Launch and Prepare the EC2 Instance

### Spin up a New EC2 Instance
- Launch a new EC2 instance in AWS using the RHEL 8 Operating System.
- Ensure that the instance is in the correct VPC and subnet with the necessary security group configurations to allow SSH access.

### Update the System
- SSH into the EC2 instance and update the system packages.

```bash
sudo yum -y update
```
![image](https://github.com/user-attachments/assets/c1345e17-4ce5-446d-81a4-aae109471b5b)

![image](https://github.com/user-attachments/assets/04a56574-9faa-405c-a382-feca5ae40095)

## Configure LVM on the Server

### Attach Volumes to the EC2 Instance

Attach all three volumes one by one to your Web Server EC2 instance.

![image](https://github.com/user-attachments/assets/4a28dbb2-f21f-4901-91e1-8e60472c5170)

### Inspect Block Devices

Use the `lsblk` command to inspect what block devices are attached to the server. Notice the names of your newly created devices. All devices in Linux reside in the `/dev/` directory. Inspect it with `ls /dev/` and make sure you see all 3 newly created block devices there - their names will likely be `xvdf`, `xvdh`, and `xvdg`.

```sh
lsblk
ls /dev/
```
![image](https://github.com/user-attachments/assets/fcfaca86-47f2-4ff0-ba9a-7bc7b15577cf)

### View Mounts and Free Space

Use the `df -h` command to see all mounts and free space on your server.

```sh
df -h
```
![image](https://github.com/user-attachments/assets/aa805f69-d91f-4a64-b8b0-fa4bb93d7f52)

### Install `lvm2` Package

Install the `lvm2` package and check for available partitions.

```sh
sudo yum install lvm2
sudo lvmdiskscan
```
![image](https://github.com/user-attachments/assets/60664152-5f7b-4d4b-9f1f-f50bcd3811e2)

![image](https://github.com/user-attachments/assets/4a2a4c1f-760f-4a1c-afb2-a00bd1e0e205)

### Create Physical Volumes
- Identify the additional disks attached to your EC2 instance and create physical volumes (PVs) using `pvcreate`.

```sh
sudo pvcreate /dev/xvdb1
sudo pvcreate /dev/xvdc1
sudo pvcreate /dev/xvdd1
```
![image](https://github.com/user-attachments/assets/e141711b-f2a6-40f2-a64e-94cd3602c1a1)

### Create a Volume Group
- Create a Volume Group (VG) named `web_vg` that includes the physical volumes.

```bash
sudo vgcreate web_vg /dev/xvdb /dev/xvdc /dev/xvdd
```

### Create Logical Volumes
- Create the following logical volumes within the volume group:
  - `lv-apps` (e.g., 10GB)
  - `lv-logs` (e.g., 20GB)
  - `lv-opt` (e.g., 5GB)

```bash
sudo lvcreate -L 10G -n lv-apps web_vg
sudo lvcreate -L 20G -n lv-logs web_vg
sudo lvcreate -L 5G -n lv-opt web_vg
```

### Format the Logical Volumes with XFS
- Format the logical volumes using the XFS filesystem.

```bash
sudo mkfs.xfs /dev/web_vg/lv-apps
sudo mkfs.xfs /dev/web_vg/lv-logs
sudo mkfs.xfs /dev/web_vg/lv-opt
```

## Create Mount Points and Mount the Volumes

### Create Mount Points
- Create directories in the `/mnt` directory for each logical volume.

```bash
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```

### Mount the Logical Volumes
- Mount the logical volumes to their respective directories.

```bash
sudo mount /dev/web_vg/lv-apps /mnt/apps
sudo mount /dev/web_vg/lv-logs /mnt/logs
sudo mount /dev/web_vg/lv-opt /mnt/opt
```

### Persist the Mounts
- Add the mount configurations to `/etc/fstab` to ensure the volumes are mounted automatically after a reboot.

```bash
echo '/dev/web_vg/lv-apps /mnt/apps xfs defaults 0 0' | sudo tee -a /etc/fstab
echo '/dev/web_vg/lv-logs /mnt/logs xfs defaults 0 0' | sudo tee -a /etc/fstab
echo '/dev/web_vg/lv-opt /mnt/opt xfs defaults 0 0' | sudo tee -a /etc/fstab
```

## Install and Configure NFS Server

### Install NFS Server
- Install the NFS utilities package.

```bash
sudo yum install nfs-utils -y
```
![image](https://github.com/user-attachments/assets/8d52caf8-5901-45c0-94ed-bfb20d8303da)

### Start and Enable NFS Server
- Start the NFS server and enable it to start on boot.

```bash
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![image](https://github.com/user-attachments/assets/a407d534-4887-4aed-a9b4-a8366041b856)

## Configure NFS Exports

### Set Permissions
- Set the correct ownership and permissions on the directories to be shared.

```bash
sudo chown -R nobody:nobody /mnt/apps
sudo chown -R nobody:nobody /mnt/logs
sudo chown -R nobody:nobody /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt
```

### Configure Exports
- Edit the `/etc/exports` file to export the directories over NFS to the specific subnet CIDR (replace `<Subnet-CIDR>` with the actual subnet CIDR, e.g., `172.31.32.0/20`).

```bash
sudo nano /etc/exports

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

# Save and exit the editor (Ctrl+O, Enter, Ctrl+X)
```

### Apply Export Changes
- Apply the NFS export changes.

```bash
sudo exportfs -arv
```
![image](https://github.com/user-attachments/assets/82145540-f534-4300-bea7-0b4c60cdd708)

## Configure Security Groups

### Open NFS Ports
- Check which ports NFS is using and open them in your EC2 security group.

```bash
rpcinfo -p | grep nfs
```

- Ensure the following ports are open in your security group:
  - TCP 111
  - UDP 111
  - UDP 2049
    
![image](https://github.com/user-attachments/assets/b34e5140-f529-424a-9980-3930c574ee49)

## Final Verification

### Restart NFS Server
- Restart the NFS server to ensure all configurations are applied correctly.

```bash
sudo systemctl restart nfs-server.service
```

### Verify NFS Exports
- Verify that the directories are correctly exported.

```bash
showmount -e
```

Now your NFS server is ready and configured to share directories with web servers and Jenkins within the specified subnet. The NFS server will automatically start on reboot, and the logical volumes will be mounted as per the configuration.

