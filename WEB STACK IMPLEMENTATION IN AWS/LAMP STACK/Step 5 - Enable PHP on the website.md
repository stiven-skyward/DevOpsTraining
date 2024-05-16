# DevOpsTraining
**DevOps/Cloud Training Material**

# Enabling PHP on Your Website

This guide provides a step-by-step process for enabling PHP on your website using Apache on Ubuntu.

## Step 5 — Enable PHP on the Website

With the default `DirectoryIndex` settings on Apache, a file named `index.html` will always take precedence over an `index.php` file. This is useful for setting up maintenance pages in PHP applications by creating a temporary `index.html` file containing an informative message to visitors. Because this page will take precedence over the `index.php` page, it will then become the landing page for the application. Once maintenance is over, the `index.html` is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the `/etc/apache2/mods-enabled/dir.conf` file and change the order in which the `index.php` file is listed within the `DirectoryIndex` directive:

```bash
$ sudo vim /etc/apache2/mods-enabled/dir.conf
```

Modify the file as follows:

```apache
<IfModule mod_dir.c>
    # Change this:
    # DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
    # To this:
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

After saving and closing the file, you will need to reload Apache so the changes take effect:

```bash
$ sudo systemctl reload apache2
```

### Creating a PHP Test Script

Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.

Create a new file named index.php inside your custom web root folder:

```bash
$ vim /var/www/projectlamp/index.php
```

This will open a blank file. Add the following text, which is valid PHP code, inside the file:

```php
<?php
phpinfo();
?>
```

When you are finished, save and close the file. Refresh the page in your web browser, and you will see a page similar to this:


This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.

If you can see this page in your browser, then your PHP installation is working as expected.

### Removing the PHP Test Script

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to do so:

```bash
$ sudo rm /var/www/projectlamp/index.php
```

