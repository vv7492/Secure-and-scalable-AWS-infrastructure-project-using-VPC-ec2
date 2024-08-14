# Setting Up a VPC with Public and Private Subnets with NAT Gateway in AWS

This guide provides step-by-step instructions for creating a VPC in AWS with both public and private subnets. We will configure a NAT Gateway to enable internet access for instances in the private subnet and demonstrate how to SSH from a public instance to a private instance.

## Steps

### Step 1: Create a VPC

1. Navigate to the **VPC Dashboard** in the AWS Management Console.
2. Click on **Create VPC**.
3. Fill out the following details:
   - **Name tag**: Enter a name for your VPC (e.g., `MyVPC`).
   - **IPv4 CIDR block**: Use `10.0.0.0/16`.
4. Click **Create VPC**.

### Step 2: Create Subnets

#### Public Subnet

1. Navigate to **Subnets** in the VPC Dashboard.
2. Click on **Create subnet**.
3. Fill out the following details:
   - **Name tag**: Enter a name for your public subnet (e.g., `PublicSubnet`).
   - **VPC**: Select the VPC created in Step 1.
   - **Availability Zone**: Choose an availability zone (e.g., `us-east-1a`).
   - **IPv4 CIDR block**: Use `10.0.0.0/24`.
   - **Auto-assign public IPv4 address**: Enable this option.
4. Click **Create subnet**.

#### Private Subnet

1. Navigate to **Subnets** in the VPC Dashboard.
2. Click on **Create subnet**.
3. Fill out the following details:
   - **Name tag**: Enter a name for your private subnet (e.g., `PrivateSubnet`).
   - **VPC**: Select the same VPC created in Step 1.
   - **Availability Zone**: Choose the same availability zone as the public subnet.
   - **IPv4 CIDR block**: Use `10.0.1.0/24`.
   - **Auto-assign public IPv4 address**: Disable this option.
4. Click **Create subnet**.

### Step 3: Launch Instances

#### Launch an EC2 Instance in the Private Subnet

1. Navigate to **Instances** in the EC2 Dashboard.
2. Click on **Launch Instance**.
3. Choose an Amazon Machine Image (AMI) and instance type.
4. Configure the instance:
   - **Network**: Select the VPC created in Step 1.
   - **Subnet**: Choose the private subnet (`10.0.1.0/24`).
   - **Auto-assign Public IP**: Disable this option.
5. Configure storage, security groups, and key pairs as needed.
6. Click **Launch**.

#### Launch an EC2 Instance in the Public Subnet

1. Navigate to **Instances** in the EC2 Dashboard.
2. Click on **Launch Instance**.
3. Choose an Amazon Machine Image (AMI) and instance type.
4. Configure the instance:
   - **Network**: Select the VPC created in Step 1.
   - **Subnet**: Choose the public subnet (`10.0.0.0/24`).
   - **Auto-assign Public IP**: Enable this option.
5. Configure storage, security groups, and key pairs as needed.
6. Click **Launch**.

### Step 4: Set Up a NAT Gateway for Internet Access

1. Navigate to the **VPC Dashboard**.
2. Click on **NAT Gateways** and then **Create NAT Gateway**.
3. Fill out the following details:
   - **Subnet**: Choose the public subnet (`10.0.0.0/24`).
   - **Elastic IP Allocation**: Select or allocate an Elastic IP address.
4. Click **Create NAT Gateway**.

### Step 5: Update Route Tables

1. Navigate to **Route Tables** in the VPC Dashboard.
2. Identify the route table associated with your private subnet (`10.0.1.0/24`).
3. Edit the route table:
   - **Destination**: `0.0.0.0/0`
   - **Target**: Select the NAT Gateway created in Step 4.
4. Save the changes.

### Step 6: Verify Internet Access

1. Launch an EC2 instance in the private subnet (`10.0.1.0/24`).
2. SSH into the EC2 instance launched in the public subnet (`10.0.0.0/24`):

   ```bash
   ssh -i your-key.pem ec2-user@public-ip-address
