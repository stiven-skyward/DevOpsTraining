# DevOpsTraining
**DevOps/Cloud Training Material**

### Step 2 - Prepare the Database Server

1. **Launch a second RedHat EC2 instance** that will have the role of 'DB Server'.
2. **Repeat the same steps as for the Web Server**, but instead of `apps-lv`, create `db-lv` and mount it to the `/db` directory instead of `/var/www/html/`.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/063f4d93-4d4b-420c-9fdf-5042c5e83327)

### Step 3 — Install WordPress on your Web Server EC2

1. **Update the repository**:
    ```sh
    sudo yum -y update
    ```

2. **Install wget, Apache, and its dependencies**:
    ```sh
    sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
    ```

3. **Start Apache**:
    ```sh
    sudo systemctl enable httpd
    sudo systemctl start httpd
    ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/8c0e00e0-814a-4ef4-85ed-3c3c44f93a08)

4. **To install PHP and its dependencies**:
    ```sh
    sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
    sudo yum module list php
    sudo yum module reset php
    sudo yum module enable php:remi-7.4
    sudo yum install php php-opcache php-gd php-curl php-mysqlnd
    sudo systemctl start php-fpm
    sudo systemctl enable php-fpm
    sudo setsebool -P httpd_execmem 1
    ```

5. **Restart Apache**:
    ```sh
    sudo systemctl restart httpd
    ```

6. **Download WordPress and copy it to /var/www/html**:
    ```sh
    mkdir wordpress
    cd wordpress
    sudo wget http://wordpress.org/latest.tar.gz
    sudo tar xzvf latest.tar.gz
    sudo rm -rf latest.tar.gz
    cp wordpress/wp-config-sample.php wordpress/wp-config.php
    cp -R wordpress /var/www/html/
    ```

7. **Configure SELinux Policies**:
    ```sh
    sudo chown -R apache:apache /var/www/html/wordpress
    sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
    sudo setsebool -P httpd_can_network_connect=1
    ```

### Step 4 — Install MySQL on your DB Server EC2

1. **Update the repository**:
    ```sh
    sudo yum update
    ```

2. **Install MySQL Server**:
    ```sh
    sudo yum install mysql-server
    ```

3. **Verify that the service is up and running**:
    ```sh
    sudo systemctl status mysqld
    ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/89c77710-8a88-4d3d-8755-f607a2a6411a)

4. **If it is not running, restart the service and enable it**:
    ```sh
    sudo systemctl restart mysqld
    sudo systemctl enable mysqld
    ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/e3476049-516f-4c3c-ba95-ba8bc4194972)

### Step 5 — Configure DB to work with WordPress

1. **Log in to MySQL**:
    ```sh
    sudo mysql
    ```

2. **Create a database and user for WordPress**:
    ```sql
    CREATE DATABASE wordpress;
    CREATE USER 'myuser'@'<Web-Server-Private-IP-Address>' IDENTIFIED BY 'mypass';
    GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
    FLUSH PRIVILEGES;
    SHOW DATABASES;
    exit;
    ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/df79f33e-c23a-4715-95aa-66344403f53e)

### Step 6 — Configure WordPress to connect to remote database

1. **Open MySQL port 3306 on DB Server EC2**. Allow access ONLY from your Web Server's IP address.
2. **Install MySQL client on the Web Server and test connection**:
    ```sh
    sudo yum install mysql
    sudo mysql -u myuser -p -h <DB-Server-Private-IP-address>
    ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f4d27008-0cd5-4896-8170-ae398ac6c79b)

3. **Verify connection**:
    ```sql
    SHOW DATABASES;
    ```

4. **Edit the wp-config.php file in the WordPress Web-Server ec2 instance**:
    - Open the file and edit.
    ```sh
    sudo nano /var/www/html/wordpress/wp-config.php
    ```
    - Change the default database connection details:
    ```
    define('DB_NAME', 'wordpress');
    define('DB_USER', 'ec2-user');
    define('DB_PASSWORD', 'mypass');
    define('DB_HOST', '172.31.27.112');  // WebServer private IP
    ```

5. **Change permissions and configuration so Apache can use WordPress**:
    - Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2.

6. **Access your WordPress installation**:
    - Open your browser and navigate to `http://<Web-Server-Public-IP-Address>/wordpress/`.

7. **If successful, you should see a screen for choosing the language and configuring the wordpress installation**

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/e0a60713-6169-463b-8e28-a192e99dc3d2)

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f47dac0a-93e6-4cf3-ba6e-dc25bd8461f4)

### Important: Do not forget to STOP your EC2 instances after completion of the project to avoid extra costs.
