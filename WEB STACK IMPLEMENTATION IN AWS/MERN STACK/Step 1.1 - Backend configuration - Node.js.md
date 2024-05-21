# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 1 - Backend Configuration

## Update and Upgrade Ubuntu

First, update your package index and upgrade your system:

```sh
sudo apt update
sudo apt upgrade
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/5a9a8672-69f5-482d-85d8-1f9f6acd63e0)

## Install Node.js

Let's get the location of Node.js software from Ubuntu repositories:

```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/ddcbf17b-71fa-441c-8776-d748a1c02c88)

Install Node.js on the server:

```sh
sudo apt-get install -y nodejs
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/c534596f-6206-4d87-be81-7e3f52eb0e9d)

> **Note:** The command above installs both `nodejs` and `npm`. `npm` is a package manager for Node.js similar to `apt` for Ubuntu. It is used to install Node.js modules & packages and to manage dependency conflicts.

Verify the Node.js installation:

```sh
node -v
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/469dbeef-bd14-474d-b456-e641f484d601)

Verify the npm installation:

```sh
npm -v
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/2a5b76b3-dc06-4135-ab15-429519b70fa9)

## Application Code Setup

Create a new directory for your To-Do project:

```sh
mkdir Todo
```

Run the command below to verify that the `Todo` directory is created:

```sh
ls
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/cef05190-f775-405a-aad1-25fa4591fb6d)

> **TIP:** In order to see some more useful information about files and directories, you can use the following combination of keys: `ls -lih`. It will show you different properties and sizes in a human-readable format. You can learn more about different useful keys for the `ls` command with `ls --help`.

Change your current directory to the newly created one:

```sh
cd Todo
```

Next, initialize your project with npm to create a `package.json` file. This file will contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press `Enter` several times to accept default values, then accept to write out the `package.json` file by typing `yes`.

```sh
npm init
```

Run the command `ls` to confirm that you have the `package.json` file created:

```sh
ls
```

You should see the `package.json` file listed in the output.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f3c3abe1-3775-41f6-928a-325a8a209b4e)
