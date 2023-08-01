# CS 1.6 Server Setup on AWS EC2 Instance

This repository contains instructions on how to set up and run a CS 1.6 server on an AWS EC2 instance. Follow the steps below to get your server up and running.

## Prerequisites

Before you begin, ensure you have the following:

- An AWS account with an EC2 instance already set up.
- Basic familiarity with working on the Linux command-line interface.

## Step-by-Step Setup

1. **SSH into your EC2 Instance**

   - Open your terminal (on Linux/macOS) or use an SSH client (e.g., PuTTY on Windows).
   - Run this command, if necessary, to ensure your key is not publicly viewable.
     ```
     chmod 400 *your-ec2-key-pair*.pem
     ```
   - Connect to your EC2 instance using the public IP address provided by AWS:
     ```
     ssh -i your-aws-key.pem ec2-user@your-ec2-public-ip
     ```

2. **Create a New User**

   - To improve security, we'll create a new user for running the CS 1.6 server:
     ```
     sudo su
     useradd cs-server
     usermod -aG sudo cs-server
     su cs-server
     ```

3. **Create Directory and Install Dependencies**

   - Create a directory for your CS 1.6 server files and install the necessary dependencies:
     ```
     mkdir cs1.6
     sudo add-apt-repository multiverse
     sudo dpkg --add-architecture i386
     sudo apt update
     sudo apt install lib32gcc-s1 steamcmd
     ```

4. **Install CS 1.6 Server**

   - Launch SteamCMD and install the CS 1.6 server files:
     ```
     steamcmd
     login anonymous
     force_install_dir /home/cs-server/cs1.6
     app_update 90 validate
     ```

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

   - Run the following command to start the server with the desired parameters (replace `0.0.0.0` with your server's IP address):
     ```
     ./hlds_run -game cstrike +ip 0.0.0.0 +maxplayers 12 +map de_dust2
     ```

   - To start the server with a different map, use the following command:
     ```
     ./hlds_run -game cstrike +maxplayers 12 +map de_inferno
     ```

7. **Accessing Your CS 1.6 Server**

   - The CS 1.6 server is now running! Players can connect to it using the public IP address of your EC2 instance and the port specified for the server.

## Troubleshooting

If you encounter any issues during the setup process or while running the server, refer to the troubleshooting section or open an issue in this repository.

---

This guide was created to help you set up a CS 1.6 server on an AWS EC2 instance. Please be aware of the licensing agreements and follow the terms of use for CS 1.6.

If you find any improvements or have suggestions for this guide, feel free to contribute to this repository.

Happy gaming!
