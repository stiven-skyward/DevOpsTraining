# DevOpsTraining
**DevOps/Cloud Training Material**

# Configuring a MySQL Database Server and Web Servers for a DevOps Solution

## Step 2: Configure the Database Server

### 2.1 Install MySQL Server
- Launch a new EC2 instance with the RHEL 9 Operating System.
- SSH into the instance and install MySQL server using the following commands:

```bash
sudo yum -y update
sudo yum install mysql-server -y
sudo systemctl start mysqld
sudo systemctl enable mysqld
```

### 2.2 Secure MySQL Installation
- Run the MySQL secure installation to set the root password and secure the installation:

```bash
sudo mysql_secure_installation
```
Follow the prompts to configure your MySQL instance securely.

### 2.3 Create the Database and User
- Log in to MySQL as the root user:

```bash
sudo mysql -u root -p
```

- Create the `tooling` database:

```sql
CREATE DATABASE tooling;
```

- Create a user `webaccess` that can only connect from the web server's subnet CIDR (replace `<Subnet-CIDR>` with your actual subnet CIDR, e.g., `172.31.32.0/20`):

```sql
CREATE USER 'webaccess'@'<Subnet-CIDR>' IDENTIFIED BY 'your_password';
```

- Grant permissions to `webaccess` on the `tooling` database:

```sql
GRANT ALL PRIVILEGES ON tooling.* TO 'webaccess'@'<Subnet-CIDR>';
FLUSH PRIVILEGES;
```

### 2.4 Verify the Database Configuration
- Exit MySQL:

```sql
EXIT;
```

- Your database server is now configured and ready for remote access from the web servers.

## Step 3: Prepare the Web Servers

### 3.1 Launch EC2 Instances for Web Servers
- Launch three new EC2 instances with the RHEL 9 Operating System. These instances will serve as your web servers.

### 3.2 Install NFS Client on Web Servers
- SSH into each web server instance and install the NFS client:

```bash
sudo yum install nano -y
sudo yum install nfs-utils nfs4-acl-tools -y
```

### 3.3 Mount NFS Shares
- Create the `/var/www` directory:

```bash
sudo mkdir /var/www
```

- Mount the NFS share from the NFS server (replace `<NFS-Server-Private-IP-Address>` with your NFS server’s private IP):

```bash
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
```

- Verify the NFS mount with the following command:

```bash
df -h
```
![image](https://github.com/user-attachments/assets/5d41760b-79e8-4fc8-8350-dfef2131af06)

### 3.4 Make the NFS Mount Persistent
- Open the `/etc/fstab` file to configure the NFS mount to persist after reboot:

```bash
sudo nano /etc/fstab
```

- Add the following line (replace `<NFS-Server-Private-IP-Address>` with your NFS server’s private IP):

```plaintext
<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0
```
![image](https://github.com/user-attachments/assets/48692839-06c2-4cea-a1b0-bcf775a1ca71)

### 3.5 Install Apache and PHP
- Install Apache HTTP server:

```bash
sudo yum install httpd -y
```

- Install PHP, and required PHP modules:

```bash
# Reset the PHP module if needed
sudo dnf module reset php

# Enable PHP 8.2
sudo dnf module enable php:8.2

# Install PHP 8.2 and required modules
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```

- Start and enable PHP-FPM:

```bash
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

- Adjust SELinux settings for Apache:

```bash
sudo setsebool -P httpd_execmem 1
```

### 3.6 Verify the NFS Mount and Apache Configuration
- Verify that the Apache files and directories are available in `/var/www` on all web servers. You can test by creating a file:

```bash
sudo touch /var/www/test.txt
```
![image](https://github.com/user-attachments/assets/656d3365-150d-4979-8200-9b3bdbd7a2a0)

- Check if the file `test.txt` is accessible on other web servers.

![image](https://github.com/user-attachments/assets/208bbcf0-60f4-4936-8cd8-03c2ef95a5af)

![image](https://github.com/user-attachments/assets/1732f108-c485-451d-ac38-8d874b5387a6)

### 3.7 Mount Apache Log Directory to NFS
- Locate Apache log folder and mount it to the NFS server's export for logs:

```bash
sudo mkdir /var/log/httpd
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd
```

- Make the mount persistent:

```bash
sudo nano /etc/fstab
```

- Add the following line:

```plaintext
<NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd nfs defaults 0 0
```

### 3.8 Deploy the Tooling Application
- Fork the tooling source code from StegHub GitHub Account to your GitHub account.
- Clone the repository to your web server:

```bash
cd /var/www/html
git clone https://github.com/<Your-GitHub-Username>/tooling.git .
```

- Ensure the `html` folder from the repository is deployed to `/var/www/html`.

### 3.9 Configure the Website to Connect to the Database
- Update the database connection settings in the `functions.php` file located in `/var/www/html`:

```bash
sudo nano /var/www/html/functions.php
```

- Apply the `tooling-db.sql` script to your database:

```bash
mysql -h <database-private-ip> -u webaccess -p tooling < tooling-db.sql
```

### 3.10 Create Admin User in MySQL
- Log in to MySQL and create a new admin user:

```sql
INSERT INTO 'users' ('id', 'username', 'password', 'email', 'user_type', 'status') VALUES (1, 'myuser', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');
```

### 3.11 Final Setup and Verification
- Open port 80 on your web server’s security group.
- Open the website in your browser using the web server’s public IP or DNS:

```plaintext
http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php
```

- Log in with the `myuser` credentials.

Congratulations! You have successfully implemented a web solution for a DevOps team using the LAMP stack with remote Database and NFS servers.
```
