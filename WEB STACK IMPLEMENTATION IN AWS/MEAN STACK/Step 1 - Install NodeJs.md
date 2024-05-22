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
curl -sL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/dc2922d7-fbc6-43c6-b191-7f37652eda29)

## Install Node.js

Finally, install Node.js and npm (Node Package Manager):

```sh
sudo apt install -y nodejs
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/e217decc-160d-44d6-a148-19afe8d6b29a)

You can verify the installation by checking the versions of Node.js and npm:

```sh
node -v
npm -v
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/5afa36b0-a484-48fb-bc8b-d9442fd71ee3)

By following these steps, you will have Node.js installed on your Ubuntu system.
