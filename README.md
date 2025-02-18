# Setup a Free VPN Server in AWS

This guide will walk you through the process of setting up a free VPN server using an EC2 instance in AWS.

## Prerequisites

- An AWS account
- Basic knowledge of AWS Management Console
- SSH client for connecting to your EC2 instance

## Step 1: Sign in to the AWS Management Console

1. Open the AWS Management Console.
2. Sign in using your AWS account credentials.
3. Set the default AWS Region to **US East (N. Virginia) us-east-1**.

## Step 2: Launching an EC2 Instance

1. Make sure you are in the **N.Virginia** Region.
2. Navigate to **EC2** by clicking on the **Services** menu at the top, then click on **EC2** under the Compute section.
3. Navigate to **Instances** on the left panel and click on the **Launch Instances** button.
4. Enter **Name** as **MyVPNServer**.
5. Choose an Amazon Machine Image (AMI):
    - Click on **Browse more AMIs**.
    - Search for **Openvpn** in the search box.
    - Click on the **Select** button of the **OpenVPN Access Server**.
    - Click on the **Continue** button in the popup window.
6. Choose an **Instance Type**:
    - Enter **t2.micro**.
7. Create a new key pair:
    - Key pair name: **MyVPNKey**
    - Key Pair Type: **RSA**
    - Private key file format: **.pem**
    - Click on the **Create key pair** button to download the key to your local machine.
8. Under **Network Settings**:
    - The following ports will be automatically enabled: 
        - SSH (22)
        - CUSTOMTCP (943, 443)
        - CUSTOMUDP (1194)
        - HTTP (80, 443)
9. Click on the **Launch Instances** button.
10. Wait for the instance to initialize and its status to change to **Running**.
11. Click on the **Instance ID** and copy the **IPv4 Public IP** of this instance.

## Step 3: SSH into EC2 Instance

1. Open your SSH client.
2. Connect to the instance using the following command:
    ```sh
    ssh -i "MyVPNKey.pem" openvpnas@<IPv4 Public IP>
    ```

## Step 4: Initialize the VPN Server

1. Enter `yes` when prompted for argument confirmation.
2. Press **ENTER** for the default responses for the following prompts:
    - Primary Access Server node
    - Network interface and IP address
    - Public/private type/algorithms for the OpenVPN CA
    - Key size for the certificates
    - Public/private type/algorithms for the self-signed web certificate
    - Key size for the certificates
    - Port number for the Admin Web UI
    - TCP port number for the OpenVPN Daemon
    - Client traffic routed by default through the VPN
    - Client DNS traffic routed by default through the VPN
    - Private subnets accessible to clients by default
3. Type a password for the 'openvpn' account: **Whizvpn123@** and confirm.
4. Press **ENTER** for the Activation key prompt.

You will see the initial configuration complete message and the Admin UI URL.

## Step 5: Connect to the VPN

1. Open a web browser and navigate to the Admin UI URL: `https://<IPv4 Public IP>:943/admin/`
2. You may get a security warning; proceed to the website.
3. Log in using:
    - Username: **openvpn**
    - Password: **Whizvpn123@**
4. Agree to the License Agreement.
5. In the VPN Settings, set **Should client Internet traffic be routed through the VPN?** to **Yes**.
6. Save the settings.

## Step 6: Client Connection

1. Open a new browser tab and navigate to `https://<IPv4 Public IP>/`
2. Log in using the credentials:
    - Username: **openvpn**
    - Password: **Whizvpn123@**
3. Download and install the VPN connector based on your operating system.
4. Open the OpenVPN Connector application and agree to the terms.
5. Turn on the pre-configured VPN profile and enter the credentials again.
6. You are now connected to the VPN.
