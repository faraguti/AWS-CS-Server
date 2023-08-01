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
   <img src="https://github.com/faraguti/cs1.6-server/blob/main/media/aws-login.png" alt="Alt Text" width="800" height="400">

2. **Navigate to EC2 Dashboard:**

   - Once logged in, navigate to the EC2 Dashboard by clicking on "Services" in the top menu and selecting "EC2" under "Compute".

3. **Launch Instance:**

   - Click on "Launch Instance" to start the instance creation process.

4. **Choose an Amazon Machine Image (AMI):**

   - Select an Ubuntu 22.04 LTS AMI or any other Ubuntu AMI you prefer.

5. **Choose an Instance Type:**

   - Select the instance type based on your requirements (e.g., t2.micro).

6. **Configure Instance Details:**

   - You can keep the default settings for most options. However, make sure to configure the following:
     - **Network**: Choose the VPC where your EC2 instance will reside.
     - **Subnet**: Choose a subnet within the selected VPC.
     - **Auto-assign Public IP**: Select "Enable" to allow your instance to have a public IP address.

7. **Add Storage:**

   - Define the storage size and type (e.g., General Purpose SSD) as per your needs.

8. **Add Tags:**

   - Optionally, you can add tags to help identify your instance (e.g., "CS-Server").

9. **Configure Security Group:**

   - Create a new security group or use an existing one to define inbound and outbound rules for your server. Make sure to open port 22 for SSH access and the port you'll be using for your CS 1.6 server (e.g., 27015) for both TCP and UDP traffic.

10. **Review and Launch:**

    - Review your instance configuration and click "Launch" when ready.

11. **Select a Key Pair:**

    - Choose an existing key pair or create a new one to allow SSH access to your instance.

12. **Launch Instance:**

    - Click "Launch Instances" to create your EC2 instance.


## Setup CS 1.6 Server

1. **SSH into your EC2 Instance**

   - Open your terminal (on Linux/macOS) or use an SSH client (e.g., PuTTY on Windows).
   - Run this command, if necessary, to ensure your key is not publicly viewable.
     ```
     chmod 400 your-ec2-key-pair.pem
     ```
   - Connect to your Ubuntu 22.04 EC2 instance using the public IP address provided by AWS:
     ```
     ssh -i your-ec2-key-pair.pem ubuntu@your-ec2-public-ip
     ```

   > :warning: **Replace "your-ec2-key-pair.pem" with the filename of your EC2 key pair and "your-ec2-public-ip" with your EC2 instance's public IP address.**

  
2. **Create a New User (Optional)**

   To improve security, we'll create a new user for running the CS 1.6 server:

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

3. **Create Directory and Install Dependencies**

   - Create a directory for your CS 1.6 server files and install the necessary dependencies:
     ```
     mkdir /home/cs-server/cs1.6
     sudo add-apt-repository multiverse
     sudo dpkg --add-architecture i386
     sudo apt update
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
     cd /home/cs-server/cs1.6
     ```

   - Create the required configuration files:
     ```
     touch cstrike/listip.cfg
     touch cstrike/banned.cfg
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
