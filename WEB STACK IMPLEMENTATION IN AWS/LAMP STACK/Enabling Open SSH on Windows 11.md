# DevOpsTraining
**DevOps/Cloud Training Material**

# Enabling Open SSH on Windows 11

## Introduction
This guide provides step-by-step instructions for installing OpenSSH on Windows using PowerShell. It also covers how to resolve permission issues with `.pem` key files.

## Installation of OpenSSH
Depending on your version of Windows, you may need to install OpenSSH using PowerShell with the NuGet Package Manager or via the Chocolatey Package Manager. To install it using PowerShell, execute the following commands in PowerShell as an administrator:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

After completing these steps, OpenSSH should be enabled on your Windows system. You can verify its status by opening PowerShell and typing the `ssh` or `ssh-keygen` commands to confirm their availability.
```

## Handling Permission Errors with `.pem` Keys

When attempting to use your .pem key, you may encounter an error regarding bad permissions:

```powershell
PS C:\Path\To\Keys> ssh -i My-Keys.pem username@example.com
Bad permissions. Try removing permissions for user: NT AUTHORITY\Authenticated Users (S-1-5-11) on file C:/Path/To/Keys/My-Keys.pem.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for 'My-Keys.pem' are too open.
This private key will be ignored.
Load key "My-Keys.pem": bad permissions
username@example.com: Permission denied (publickey).
```
This error indicates that the permissions for the `My-Keys.pem` file are too open, which means that other system users might have the ability to access this file. For security reasons, SSH requires that private keys be accessible only by the file's owner and no one else.

## Resolving Permission Issues on Windows

Open PowerShell as an administrator and perform the following steps:

1. **Reset the permissions of the file:**
 ```powershell
   - icacls C:\Path\To\Keys\My-Keys.pem /reset
   ```
