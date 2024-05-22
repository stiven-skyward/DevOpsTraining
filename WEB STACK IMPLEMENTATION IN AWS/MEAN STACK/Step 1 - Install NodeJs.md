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
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/cc570bca-2c5a-47d4-a1eb-e3008ea74e75)

## Add Certificates

Install necessary certificates and tools for fetching and verifying Node.js packages:

```sh
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f6ad5cb8-5f7f-4e34-b587-87aa4bfc2090)

## Add Node.js Repository

Use the NodeSource setup script to add the Node.js 20.x repository to your system:

```sh
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/77f45171-ff4b-4b73-b365-53f2dc80e970)

## Install Node.js

Finally, install Node.js and npm (Node Package Manager):

```sh
sudo apt install -y nodejs
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/64506135-affe-49cb-b6e6-65b1c14c93fb)

You can verify the installation by checking the versions of Node.js and npm:

```sh
node -v
npm -v
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/5afa36b0-a484-48fb-bc8b-d9442fd71ee3)

By following these steps, you will have Node.js installed on your Ubuntu system.
