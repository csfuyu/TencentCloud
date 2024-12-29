# Establishing a VPN Connection Between Tencent Cloud and AWS

This guide walks you through the steps to create a secure VPN connection between Tencent Cloud and Amazon Web Services (AWS). The connection will allow private communication between resources hosted in both clouds.

## Prerequisites

Before proceeding, ensure you have the following:

- **Tencent Cloud Account** with necessary permissions (Admin/Network Administrator)
- **AWS Account** with access to VPN configuration
- Basic knowledge of **Tencent Cloud VPC** and **AWS VPC**
- Access to the **Tencent Cloud Console** and **AWS Management Console**
- Administrative privileges to configure the VPN on both sides

## Overview of VPN Architecture

In this section, we will describe the high-level architecture for connecting Tencent Cloud and AWS using a VPN tunnel.

- **VPC in Tencent Cloud** and **VPC in AWS** that will communicate via a VPN tunnel.
- **Cloud VPN** in Tencent Cloud and **Virtual Private Gateway** in AWS.
- The VPN tunnel will be established over the internet using IKEv2/IPsec protocols.

![](./vpn.png)

## Step 1: Setting Up the VPN Gateway in Tencent Cloud

### 1.1 Create a Virtual Private Cloud (VPC)

1. Log in to the **Tencent Cloud Console**.
2. Navigate to **VPC** and click on **Create VPC**.
3. Configure your VPC with appropriate CIDR blocks(192.168.0.0/16), subnet(192.168.2.0/24)

### 1.2 Create the VPN Gateway in Tencent Cloud

1. Go to **Network > VPN** in the Tencent Cloud Console.
2. Click **Create VPN Gateway** and choose IPSec as the protocal.
3. After creation, you can get the gateway IP.

### 1.3 Configure the VPN Tunnel (IPSec)

1. Use the configuration details obtained from AWS to set up the **VPN Tunnel** in Tencent Cloud.
2. Set the tunnel parameters such as IKE version, pre-shared keys, and encryption settings.

### 1.4 Create the Customer Gateway (CGW) in Tencent Cloud

1. In the **Tencent Cloud Console**, create a new **Customer Gateway** for the AWS side.
2. Provide the public IP address of the VPN endpoint (this will be the external IP of AWS’s Virtual Private Gateway).
3. Set the VPN routing options and authentication details.

### 1.5 Testing the VPN Tunnel

1. After configuring both sides, ensure that the VPN connection is **UP** and **active**.
2. Run **ping tests** or other connectivity checks to verify the tunnel's operation.

## Step 2: Setting Up the VPN in AWS

### 2.1 Create a Virtual Private Cloud (VPC)

1. Log in to the **AWS Management Console**.
2. Navigate to **VPC** and click on **Create VPC**.
3. Define your VPC’s CIDR block(172.31.0.0/16), subnet(172.31.0.0/20).

### 2.2 Create a Virtual Private Gateway (VGW)

1. Navigate to **VPC > Virtual Private Gateways**.
2. Click **Create Virtual Private Gateway**.
3. Attach the Virtual Private Gateway to your AWS VPC.

### 2.3 Configure the Customer Gateway (CGW) in AWS

1. In the **VPC Console**, create a new **Customer Gateway** for the Tencent Cloud side.
2. Provide the public IP address of the VPN endpoint (this will be the external IP of Tencent Cloud’s VPN Gateway).

### 2.4 Create the VPN Connection in AWS

1. Go to **VPC > Site-to-Site VPN Connections**.
2. Click **Create VPN Connection** and select the Virtual Private Gateway and Customer Gateway. Routing options as Static, local CIDR 192.168.0.0/16, remote CIDR 172.31.0.0/16. Leave Tunnel1 and Tunnel2 options blank as default.
3. After the creation. Add static routes ![](./vpn-static-routes.png)
4. Download the **VPN Configuration** file, which contains the details needed for configuring the Tencent Cloud side.

## Step 3: Create VPN Tunnel in Tencent Cloud

### 3.1 Create Tunnel 1

1. Go to **VPC > Network > VPN Tunnel** in the Tencent Cloud Console
2. Click Create
3. Configure the basic and advanced options according to the configuration file download from AWS. ![](tunnel1-basic.png) ![](tunnel1-advanced.png)

### 3.2 Create Tunnel 2

1. Repeat the steps to create Tunnel 2 

### 3.3 Configure routings in VPN Gateway

1. Go to **VPC > Network > VPN Gateway**
2. Choose the VPN Gateway created in step 1.2
3. Go to Route Table and configure the routings for two tunnels. The VPN traffic will go to one of the tunnels as configured and go to the other if the first tunnel is down.
![](tencent vpn gw route.png)


## Conclusion

In this guide, we successfully established a VPN connection between Tencent Cloud and AWS, enabling secure communication between the two cloud environments. You can now securely route traffic between your Tencent Cloud VPC and AWS VPC.

For any additional assistance, refer to the official [Tencent Cloud VPN documentation](https://intl.cloud.tencent.com/document/product/1037) and [AWS VPN documentation](https://aws.amazon.com/vpn/).

## References

- [Tencent Cloud VPN Documentation](https://intl.cloud.tencent.com/document/product/1037)
- [AWS VPN Documentation](https://aws.amazon.com/vpn/)