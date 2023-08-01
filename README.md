![image](https://github.com/faraguti/cs1.6-server/assets/5418256/01a9e382-9fd2-4a65-9678-bb6094ae93bf)

# CS 1.6 Server Setup on AWS EC2 Instance (Ubuntu 22.04)

This repository contains instructions on how to set up and run a CS 1.6 server on an AWS EC2 instance running Ubuntu 22.04. Follow the steps below to get your server up and running.

## Prerequisites

Before you begin, ensure you have the following:

- Basic familiarity with working on the Linux command-line interface.

## Summary

1. [Create an EC2 Instance on AWS](#creating-an-ec2-instance-on-aws)
2. [Setup CS 1.6 Server](#setup-cs-16-server)

## Creating an EC2 Instance on AWS

Follow these steps to create an EC2 instance on AWS for hosting your CS 1.6 server:

1. **Sign in to AWS Console:**

   - Go to the AWS Management Console (https://aws.amazon.com/).
   - Sign in with your AWS account credentials.
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/9c820c93-8c02-44ac-a2d3-6f8a23f0d4e6" height="90%" width="90%">

2. **Navigate to EC2 Dashboard:**

   - Once logged in, navigate to the EC2 Dashboard by clicking on "Services" in the top menu and selecting "EC2" under "Compute".
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/b6e78a13-5754-494e-8b39-20c93cc5717d" height="90%" width="90%">

3. **Launch Instance:**

   - Click on "Launch Instance" to start the instance creation process.
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/4bef5437-6b96-4ea8-837a-d6425aa6a676" height="90%" width="90%">

4. **Choose an Amazon Machine Image (AMI):**

   - Select an Ubuntu 22.04 LTS AMI or any other Ubuntu AMI you prefer.
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/433c24e4-ef32-4f81-a84c-736de362d14c" height="90%" width="90%">

5. **Choose an Instance Type:**

   - Select the instance type based on your requirements (e.g., t2.micro).
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/770e9b6e-c2e8-4d65-817c-86ba615f4e0a" height="90%" width="90%">

6. **Select a Key Pair:**

    - Choose an existing key pair or create a new one to allow SSH access to your instance.
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/4c7e7d52-ff23-41b4-bc19-16d3ee23e45a" height="90%" width="90%">
 
7. **Configure Network Settings:**

   - You can keep the default settings for these options:
     - **Network**: Choose the VPC where your EC2 instance will reside.
     - **Subnet**: Choose a subnet within the selected VPC.
     - **Auto-assign Public IP**: Select "Enable" to allow your instance to have a public IP address.
      
8. **Configure Security Group:**

   - Create a new security group or use an existing one to define inbound and outbound rules for your server. Make sure to open port 22 for SSH access and the port you'll be using for your CS 1.6 server (e.g., 27015) for both TCP and UDP traffic.
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/65bd6398-3e0e-445b-8e6f-57511e966cd2" height="90%" width="90%">

   > :warning: **To improve security, consider updating the security group's inbound rule for SSH (port 22) to only allow access from your specific IP address. This way, only your IP will be able to SSH into the instance, reducing the risk of unauthorized access**

   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/d9538453-f8b2-471c-8207-eeab1d297570" height="90%" width="90%">
   
   > :warning: **Opening both TCP and UDP ports 27015 (used by the CS 1.6 server) to "0.0.0.0" (any IP address) allows any player to connect to your server.**

9. **Add Storage:**

   - Define the storage size and type (20GiB is enough).
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/73b3fb15-cbbb-482b-9a83-8658f5d12610" height="90%" width="90%">

10. **Review and Launch Instance:**

    - Review your instance configuration and click "Launch Instance" when ready.

<br><br/>

## Setup CS 1.6 Server

1. **SSH into your EC2 Instance**

   - Open your terminal (on Linux/macOS), PowerShell (on Windows/macOS/Linux) or use an SSH client (e.g., PuTTY on Windows).
   - Run these commands, if necessary, to ensure your key is not publicly viewable.

     **Linux/macOS**
      ```
     chmod 400 your-ec2-key-pair.pem
     ```
      
     **Windows**<br>
     Replace 'C:\path\to\private_key.pem' with the actual path to your private key file
     ```
      $pathToPrivateKey = 'C:\path\to\private_key.pem'
      ```
     Set restrictive permissions (Read and Write for the owner, no permissions for others)
     ```
      icacls $pathToPrivateKey /inheritance:r
      icacls $pathToPrivateKey /grant:r 'NT AUTHORITY\SYSTEM:(R)'
      icacls $pathToPrivateKey /grant:r 'BUILTIN\Administrators:(R)'
      icacls $pathToPrivateKey /remove:g '*S-1-1-0'
      ```
      
   - Connect to your Ubuntu 22.04 EC2 instance using the public IP address provided by AWS:
     ```
     ssh -i your-ec2-key-pair.pem ubuntu@your-ec2-public-ip
     ```
     
   > :warning: **Replace "your-ec2-key-pair.pem" with the filename of your EC2 key pair and "your-ec2-public-ip" with your EC2 instance's public IP address.**

   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/28987c7c-4742-414c-9975-f0b253520c34" height="90%" width="90%">


2. **Create a New User (Optional)**

   To improve security and organization, we'll create a new user for running the CS 1.6 server:

   - Switch to the superuser (root) account using the following command:
     ```
     sudo su
     ```

   - Create a new user named "cs-server" with the following command:
     ```
     adduser cs-server
     ```

   - Add the "cs-server" user to the "sudo" group to grant administrative privileges:
     ```
     usermod -aG sudo cs-server
     ```

   - Switch to the newly created "cs-server" user with the following command:
     ```
     su cs-server
     ```
   <br/>
   <img src="https://github.com/faraguti/cs1.6-server/assets/5418256/acaadb39-122a-4b0e-8ab5-c70ccdeac29b" height="90%" width="90%">
     

3. **Create Directory and Install Dependencies**

   - Create a directory for your CS 1.6 server files and install the necessary dependencies:
     ```
     mkdir /home/cs-server/cs1.6 |
     sudo add-apt-repository multiverse |
     sudo dpkg --add-architecture i386 |
     sudo apt update |
     sudo apt install lib32gcc-s1 steamcmd -y
     ```

4. **Install CS 1.6 Server**

   - Launch SteamCMD to download and install the CS 1.6 server files:
     ```
     steamcmd
     ```

   - Log in to Steam anonymously to access the necessary CS 1.6 server files:
     ```
     login anonymous
     ```

   - Set the installation directory to `/home/cs-server/cs1.6`:
     ```
     force_install_dir /home/cs-server/cs1.6
     ```

   - Download and install the CS 1.6 server files (App ID 90) and validate the installation:
     ```
     app_update 90 validate
     ```
   > :warning: **Note: The installation process may take some time, and it's possible that it could fail or stop due to various reasons, such as network interruptions or server load. If the installation process fails, don't worry; simply run the command again until the installation is successful.**


5. **Prepare Server Configurations**

   - Change the directory to the CS 1.6 server folder:
     ```
     cd /home/cs-server/cs1.6/cstrike
     ```

   - Create the required configuration files:
     ```
     touch listip.cfg
     touch banned.cfg
     ```

6. **Start the CS 1.6 Server**

   - Run the following command to start the server with the desired parameters:
     ```
     ./hlds_run -game cstrike +maxplayers 12 +map de_dust2
     ```

   > :warning: **Note:** The first time you run the server after installation, you may encounter a "FATAL ERROR (shutting down): Unable to initialize Steam" error, and the server won't start successfully. This issue is common after the initial installation. To resolve it, simply run the same command again, and the server should start without any issues on the second attempt. Subsequent server starts should work smoothly.

7. **Accessing Your CS 1.6 Server**

   - The CS 1.6 server is now running! Players can connect to it using the public IP address of your EC2 instance and the port specified for the server.

## Troubleshooting

If you encounter any issues during the setup process or while running the server, refer to the troubleshooting section or open an issue in this repository.

---

This guide was created to help you set up a CS 1.6 server on an AWS EC2 instance. Please be aware of the licensing agreements and follow the terms of use for CS 1.6.

If you find any improvements or have suggestions for this guide, feel free to contribute to this repository.

Happy gaming!
