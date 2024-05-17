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

You can change the `.pem` file permissions using Windows Command Prompt with administrative privileges:

1. **Open Command Prompt as Administrator**:
   - *Click on the Start menu, type `cmd`.*
   - *Right-click on `Command Prompt` and select `Run as administrator`.*

2. **Navigate to the Directory**:
   - ```cmd
     cd /Path-to-Your-private-key.pem/
     ```

3. **Change File Permissions**:
   - ```cmd
     icacls My-Keys.pem /inheritance:r /grant:r %username%:R
     ```

Launch Git Bash as administrator and run the following commands to connect to your EC2 instance:

```sh
cd /Path-to-Your-private-key.pem/
```

```sh
chmod 400 Your-private-key.pem
```

```sh
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```

Replace `<Your-private-key.pem>` with the path to your private key file and `<EC2-Public-IP-address>` with the public IP address of your EC2 instance.

### Example Command

If your private key file is named `my-key.pem` and your EC2 instance's public IP address is `192.0.2.0`, the command would look like this:

```sh
ssh -i my-key.pem ubuntu@192.0.2.0
```

By using Git Bash, you can avoid the need to convert your `.pem` key to a `.ppk` key, simplifying the process of connecting to your EC2 instance.
