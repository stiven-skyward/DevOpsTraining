# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 1: Install Node.js

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

## Update Ubuntu

First, update the package index to ensure you have the latest information about available packages:

```sh
sudo apt update
```

## Upgrade Ubuntu

Next, upgrade the installed packages to their latest versions:

```sh
sudo apt upgrade
```

## Add Certificates

Install necessary certificates and tools for fetching and verifying Node.js packages:

```sh
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
```

## Add Node.js Repository

Use the NodeSource setup script to add the Node.js 12.x repository to your system:

```sh
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```

## Install Node.js

Finally, install Node.js and npm (Node Package Manager):

```sh
sudo apt install -y nodejs
```

You can verify the installation by checking the versions of Node.js and npm:

```sh
node -v
npm -v
```

By following these steps, you will have Node.js installed on your Ubuntu system.