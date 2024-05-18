# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 2 - Installing MySQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

## Installing MySQL

Use `apt` to acquire and install MySQL:

```sh
sudo apt install mysql-server
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/a362d538-ca42-4c5a-863e-23429eb6dad2)

When prompted, confirm installation by typing `Y`, and then `ENTER`.

## Logging In to MySQL

When the installation is finished, log in to the MySQL console by typing:

```sh
sudo mysql
```

This will connect to the MySQL server as the administrative database user `root`, which is inferred by the use of `sudo` when running this command. You should see output like this:

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/97695e92-6040-4c50-9add-db30ec3eea90)

## Securing MySQL

It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system.

### Setting a Password for the Root User

Before running the script, set a password for the root user, using `mysql_native_password` as the default authentication method. We’re defining this user’s password as `PassWord.1`.

In the MySQL shell, run:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/3eb91c28-a6b5-4334-a2b6-c3de817f1bfd)

Exit the MySQL shell with:

```sql
mysql> exit
```

### Running the Security Script

Start the interactive script by running:

```sh
sudo mysql_secure_installation
```

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

#### Configuring the VALIDATE PASSWORD PLUGIN

Enabling this feature is optional. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/fa5b5673-e866-49f7-b3b1-e336de2680bd)

Press y|Y for Yes, or any other key for No:

```sh
Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
```

### Setting the Root Password

Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN, your server will next ask you to select and confirm a password for the MySQL `root` user.

If you enabled password validation, you’ll be shown the password strength for the `root` password you just entered and your server will ask if you want to continue with that password:

```sh
Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
```

For the rest of the questions, press `Y` and hit the `ENTER` key at each prompt. This will prompt you to:

- **Change the root password (if you haven't already)**
- **Remove some anonymous users**
- **Remove the test database**
- **Disable remote root logins**
- **Reload privilege tables**

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/3ff9b13d-22c2-4d01-9665-e12948dcd5fa)

## Testing MySQL Login

```sh
sudo mysql -p
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/e0b7799b-1ec1-4241-971f-4dd113e0018e)

Notice the `-p` flag in this command, which will prompt you for the password you set for the `root` user.

To exit the MySQL console, type:

```sh
mysql> exit
```

For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple databases hosted on your server.

## Note on PHP and MySQL 8

At the time of this writing, the native MySQL PHP library `mysqlnd` doesn’t support `caching_sha2_authentication`, the default authentication method for MySQL 8. For that reason, when creating database users for PHP applications on MySQL 8, you’ll need to make sure they’re configured to use `mysql_native_password` instead. We’ll demonstrate how to do that in Step 6.
