# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 3 - Installing PHP

You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install `php-fpm`, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need `php-mysql`, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

## Installing PHP

To install these two packages at once, run:

```sh
sudo apt install php-fpm php-mysql
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/2bbaf635-edd3-4390-a32e-3d138f5bb2a0)

When prompted, type `Y` and press `ENTER` to confirm installation.

You can confirm that you have your PHP components installed using the command:

```sh
php -v
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/90dd6e1a-1bd8-4882-950a-576fe480630a)
