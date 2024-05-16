# DevOpsTraining
**DevOps/Cloud Training Material**

# Apache Installation Guide

This guide provides a step-by-step process for installing and configuring Apache on Ubuntu.

## Installing Apache and Updating the Firewall

### What exactly is Apache?

Apache HTTP Server is the most widely used web server software. Developed and maintained by the Apache Software Foundation, Apache is open source software available for free. It runs on 67% of all webservers in the world. It is fast, reliable, and secure. It can be highly customized to meet the needs of many different environments by using extensions and modules. Most WordPress hosting providers use Apache as their web server software. However, websites and other applications can run on other web server software as well, such as Nginx or Microsoft's IIS.

The Apache web server is among the most popular web servers in the world. It’s well documented, has an active community of users, and has been in wide use for much of the history of the web, making it a great default choice for hosting a website.

### Installing Apache

Install Apache using Ubuntu’s package manager `apt`:

```bash
# Update a list of packages in package manager
$ sudo apt update

# Run apache2 package installation
$ sudo apt install apache2
```
To verify that apache2 is running as a service in our OS, use the following command:

```bash
$ sudo systemctl status apache2
```
If it is green and running, then you did everything correctly - you have just launched your first Web Server in the Cloud!

### Opening Port 80

Before we can receive any traffic on our web server, we need to open TCP port 80, which is the default port that web browsers use to access web pages on the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH. So, we need to add a rule to EC2 configuration to open inbound connections through port 80.

### Open inbound port 80

Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means 'from any IP address').

First, let us try to check how we can access it locally in our Ubuntu shell:

```bash
$ curl http://localhost:80
```
or
```bash
$ curl http://127.0.0.1:80
```
These two commands above actually do pretty much the same - they use the `curl` command to request our Apache HTTP Server on port 80. The difference is that in the first case we try to access our server via DNS name and in the second one - by IP address (in this case, IP address 127.0.0.1 corresponds to DNS name 'localhost' and the process of converting a DNS name to IP address is called "resolution"). We will touch DNS in further lectures and projects.

As an output, you can see some strangely formatted text. Do not worry, we just made sure that our Apache web service responds to the `curl` command with some payload.

Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet. Open a web browser of your choice and try to access the following URL:

```bash
http://<Public-IP-Address>:80
```
Another way to retrieve your Public IP address, other than to check it in the AWS Web console, is to use the following command:

```bash
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
The URL in the browser shall also work if you do not specify the port number since all web browsers use port 80 by default.

If you see the following page, then your web server is now correctly installed and accessible through your firewall:

![Apache-Default-Page](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/8aafed71-23a1-49b8-9764-3853f1b39555)

