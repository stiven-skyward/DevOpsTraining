# DevOpsTraining
**DevOps/Cloud Training Material**

# PHP Installation Guide

This guide provides a step-by-step process for installing and configuring PHP on Ubuntu.

## Step 3 — Installing PHP

You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the `php` package, you’ll need `php-mysql`, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need `libapache2-mod-php` to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

### Installing PHP

To install these three packages at once, run:

```bash
$ sudo apt install php libapache2-mod-php php-mysql
```

Once the installation is finished, you can run the following command to confirm your PHP version:

```bash
$ php -v
```
You should see output similar to the following:
```scss
PHP 7.4.3 (cli) (built: Oct  1 2020 13:57:00) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies
```
### Verifying PHP Installation

To verify that PHP is correctly installed and configured to work with Apache, you can create a simple PHP script.

1. Create a PHP File: Create a new PHP file in the web root directory. Typically, this is /var/www/html/.

```bash
$ sudo nano /var/www/html/info.php
```
2. dd PHP Info: Add the following code to the file:

```php
<?php
phpinfo();
?>
```
3. Save and Exit: Save the file and exit the text editor.

4. Test PHP in Web Browser: Open your web browser and navigate to http://your_server_IP_address/info.php.

You should see a page displaying detailed information about your PHP installation. This confirms that PHP is working correctly with Apache.

### Removing the PHP Info File

For security reasons, it’s best to remove the info.php file once you’ve verified that PHP is working correctly.

```bash
$ sudo rm /var/www/html/info.php
```
This completes the PHP installation and configuration process.

