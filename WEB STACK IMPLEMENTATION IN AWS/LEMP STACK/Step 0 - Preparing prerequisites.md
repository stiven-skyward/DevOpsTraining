# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 0 - Preparing Prerequisites

In order to complete this project, you will need an AWS account and a virtual server with Ubuntu Server OS.

## AWS Account Setup

If you do not have an AWS account, follow these steps:

1. **Sign in to AWS Free Tier Account**:
   - Go back to **Project 1 Step 0** to sign up for the AWS Free Tier account.
   - Follow the instructions to create a new account if you haven't done so already.
  
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/10fa8133-19d6-4242-b393-b8dd67406170)

2. **Create a New EC2 Instance**:
   - Navigate to the EC2 Dashboard in your AWS Management Console.
   - Click on **Launch Instance** and select **t2.micro** family.
   - Choose the **Ubuntu Server 22.04 LTS (HVM)** image.

   Remember, you can have multiple EC2 instances, but make sure you **STOP** the ones you are not working with at the moment to save available free hours.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/bcd44bdd-c1f1-4878-abdc-4c8e1555a3ff)

## Connecting to EC2 Instance using Git Bash

### Install Git Bash

Download and install Git Bash from the [official Git website](https://gitforwindows.org/).

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/0f434d5e-2925-4b31-bf19-4022a747cf9b)

### Connect to EC2 Instance

You can change the `.pem` file permissions using the Windows Subsystem for Linux (WSL):

1. **Open PowerShell as Administrator**:
   - *Click on the Start menu, type `PowerShell`.*
   - *Right-click on `PowerShell` and select `Run as administrator`.*

2. **Run the command**:
   - ```powershell
     wsl --install
     ```
   - Follow the instructions and restart your computer once the process is completed.
  
3. **Open WSL Terminal and navigate to the directory**:
   - ```sh
     cd /mnt/Path-to-Your-private-key.pem/
     ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/828666a5-94c9-4cb3-be6f-4f0e22fc5741)

4. **Copy the Key File to the WSL Home Directory**:
   - ```sh
     cp /mnt/d/DevOps/My-Keys.pem ~/
     ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/41db1959-4ef0-4d6e-a5b4-f940fea6f937)

5. **Change File Permissions**:
   - ```sh
     chmod 400 ~/My-Keys.pem
     ```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/0eec1422-44c5-46f3-b226-f007d775331d)

Launch Git Bash as administrator and run the following command to connect to your EC2 instance:

```sh
cd /Path-to-Your-private-key.pem/
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/574cd24c-d7ee-4144-a15b-c3adca554d33)

```sh
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/fa190708-568f-4dfd-86e6-ead4bd51032e)

Replace `<Your-private-key.pem>` with the path to your private key file and `<EC2-Public-IP-address>` with the public IP address of your EC2 instance.

### Example Command

If your private key file is named `my-key.pem` and your EC2 instance's public IP address is `192.0.2.0`, the command would look like this:

```sh
ssh -i my-key.pem ubuntu@192.0.2.0
```

By using Git Bash, you can avoid the need to convert your `.pem` key to a `.ppk` key, simplifying the process of connecting to your EC2 instance.
