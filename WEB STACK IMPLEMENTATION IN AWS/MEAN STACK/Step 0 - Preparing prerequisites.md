# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 0 - Preparing Prerequisites

In order to complete this project, you will need an AWS account and a virtual server with Ubuntu Server OS.

## Setting Up an AWS Account and EC2 Instance

### If you do not have an AWS account:
Go back to **Project 1 Step 0** to sign in to the AWS free tier account and create a new EC2 Instance of `t2.micro` family with Ubuntu Server 24.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you **STOP** the ones you are not working with at the moment to save available free hours.

### Creating an EC2 Instance

1. **Navigate to the EC2 Dashboard** in your AWS Management Console.
2. **Click on "Launch Instance"**.
3. **Select the Ubuntu Server 24.04 LTS (HVM) image**.
4. **Choose the t2.micro instance type**.
5. **Configure instance details**, add storage, and configure security groups as needed.
6. **Add Tags**:
   - Key: `Name`
   - Value: Corresponds to the current project you are working on (e.g., "Project X Server")

### Example Tag Configuration
```text
Key: Name
Value: Project X Server
```

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/ffcd9c76-2473-4af3-9a56-8519e3dfb384)

## Connecting to Your EC2 Instance

### Hint #1: Tagging Your Instances
When you create your EC2 Instances, you can add the Tag `Name` to it with a value that corresponds to the current project you are working on. This tag will be reflected in the name of the EC2 Instance.

### Hint #2 (for Windows users only): Using Windows Terminal
In previous projects, we used Putty and Git Bash to connect to our EC2 Instances. In this project and going forward, we are going to explore the usage of Windows Terminal.

### Steps to Connect Using Windows Terminal

1. **Download and Install Windows Terminal** from the Microsoft Store if you haven't already.
2. **Launch Windows Terminal**.
3. **Open a new tab with PowerShell or Command Prompt**.
4. **Connect to your EC2 Instance using SSH**:
   ```sh
   ssh -i "path/to/your-key.pem" ubuntu@your-ec2-public-ip
   ```

Replace `"path/to/your-key.pem"` with the path to your private key file and `your-ec2-public-ip` with the public IP address of your EC2 instance.

### Example Command
```sh
ssh -i "C:/Users/YourName/keys/my-key.pem" ubuntu@123.45.67.89
```

