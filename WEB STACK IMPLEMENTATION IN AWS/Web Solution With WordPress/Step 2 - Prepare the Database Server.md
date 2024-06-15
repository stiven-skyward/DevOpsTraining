# DevOpsTraining
**DevOps/Cloud Training Material**

### Step 2 — Prepare the Database Server

1. **Launch a second RedHat EC2 instance** that will have the role of 'DB Server'.
2. **Repeat the same steps as for the Web Server**, but instead of `apps-lv`, create `db-lv` and mount it to the `/db` directory instead of `/var/www/html/`.

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

4. **If it is not running, restart the service and enable it**:
    ```sh
    sudo systemctl restart mysqld
    sudo systemctl enable mysqld
    ```

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

### Step 6 — Configure WordPress to connect to remote database

1. **Open MySQL port 3306 on DB Server EC2**. Allow access ONLY from your Web Server's IP address.
2. **Install MySQL client on the Web Server and test connection**:
    ```sh
    sudo yum install mysql
    sudo mysql -u myuser -p -h <DB-Server-Private-IP-address>
    ```

3. **Verify connection**:
    ```sql
    SHOW DATABASES;
    ```

4. **Change permissions and configuration so Apache can use WordPress**:
    - Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2.

5. **Access your WordPress installation**:
    - Open your browser and navigate to `http://<Web-Server-Public-IP-Address>/wordpress/`.
    - Fill out your DB credentials.

6. **If successful, you should see a message confirming that WordPress has connected to your remote MySQL database.**

### Important: Do not forget to STOP your EC2 instances after completion of the project to avoid extra costs.