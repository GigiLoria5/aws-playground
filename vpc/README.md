# Create a Custom VPC with Public and Private Subnets

This guide demonstrates how to create a Virtual Private Cloud (VPC) with both public and private subnets in AWS.

## Step 1: Create a new VPC

- Go to **VPC Dashboard** in the AWS Console and choose **Create VPC**.
- Configure the following details:
  - **Name**: My-VPC
  - **IPv4 CIDR Block**: 10.0.0.0/16
- Leave the remaining options as default.

## Step 2: Create Public and Private Subnets

- In the VPC Dashboard, go to **Subnets** and select **Create subnet**.
- Choose your VPC (**My-VPC**), then configure the subnets as follows:

  ```
  Name: Public-1A
  Availability Zone: eu-south-1a
  IPv4 CIDR Block: 10.0.1.0/24

  Name: Public-1B
  Availability Zone: eu-south-1b
  IPv4 CIDR Block: 10.0.2.0/24

  Name: Private-1A
  Availability Zone: eu-south-1a
  IPv4 CIDR Block: 10.0.3.0/24

  Name: Private-1B
  Availability Zone: eu-south-1b
  IPv4 CIDR Block: 10.0.4.0/24
  ```

- Once created, for each public subnet, go to **Subnet settings**, select the **Edit subnet settings** option, and enable **Auto-assign public IPv4 address**. This setting allows instances in the public subnet to receive a public IP address automatically.

> Note: You can also enable auto-assign IP when launching an EC2 instance.

## Step 3: Create an Internet Gateway

- Go to the **Internet Gateways** section in the VPC Dashboard and choose **Create internet gateway**.
- Create one gateway by just specifying a name (e.g., My-IGW).
- After creation, select **Actions** > **Attach to VPC** and attach it to **My-VPC**.

## Step 4: Update the Public RT

- In the **Route Tables** section of the VPC Dashboard, find the main (default) route table associated with **My-VPC**. We'll use this table for the public subnets. Optionally, you can rename this route table to something like **My-Public-RT** for easier identification.
  > Note: Any subnet not explicitly associated with a route table will default to this one.
- Edit the routes for the above-mentioned table:

  - **Destination**: `0.0.0.0/0`
  - **Target**: Select the internet gateway created before (**My-IGW**).

    > In this way, all traffic not destined for the local network will be routed to our gateway.

- Associate the route table with the public subnets by going under **Subnet Associations** and selecting **Edit subnet associations**.

## Step 5: Create the Private RT

- In the **Route Tables** section, choose **Create route table**.
- Configure the following:

  - **Name**: My-Private-RT
  - **VPC**: My-VPC

- Once created, go to **Subnet Associations**, select **Edit subnet associations** and add the private subnets.

## Step 6: Test the VPC Configuration with an EC2 Instance

- Launch a new EC2 instance from the dashboard.
  - In Network settings select **My-VPC** as the VPC and one **public subnet** as the subnet.
  - Under Security group settings, select **Create a new security group** and configure an inbound rule to **open accesss** to the **HTTP port 80**.
  - In the Advanced details section, locate the **User data** field. Copy and paste the content of the `user-data-simple-website.sh` file.
- Once the instance reaches the running state, go to the **Instances** page, copy the **Public IPv4 address** of the instance and and navigate to `http://<instance-public-ip>`.
- You should see a default web page, which means that the web server is active and configured within the new VPC.
