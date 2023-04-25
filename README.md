# VPC Architecture

![Alt text](img/Amazon%20VPC.png)

## What is VPC?

- A VPC (Virtual Private Cloud) is a virtual network that closely resembles a traditional network that you'd operate in your own data center. An isolated space for privacy.

## Benefits of VPC?

- Security.
- Easy to deploy.
- Space saving.
- Seamless updates.
- Easy intergration.

## What is Internet Gateway?

- It is a VPC component that allows communication between your VPC and the internet.

## What is Subnet?

- A subnet is a range of IP addresses in your VPC and resides in a single Availabilty Zone.

## What is CIDR?

- A range of IPv4 or IPv6 addresses that you set aside so that AWS can't assign them to your network interfaces.

## What are Route Tables?

- A route table contains a set of rules, called routes, that determine where network traffic from your subnet or gateway is directed.

# Deploy Node App In VPC

## Create VPC

1. Search VPC in the search bar and click VPC.

2. Click create VPC to start a new one.

3. Select VPC only.

4. Name your VPC with your required name. I went with `samuel_tech221_VPC_1`

5. For IPv4 CIDR block select manual input and input your CIDR block. I went with `10.0.0.0/16`

6. Click orange box create VPC to launch.

## Create Internet Gateway

1. Click left hand side on Internet gateways.

2. Click create Internet gateway.

3. Name your gateway with your required name. I went with `samuel_tech221_IG_VPC`

4. Click orange box create internet gateway to launch.

5. Click actions and then attach to VPC.

6. Select the VPC you just created.

7. Click attach.

## Create Subnet

1. Click subnets on left hand side of page.

2. Click create subnet.

3. Select the VPC you just created.

4. Name the subnet with your required name. I went with `samuel_tech221_public_app_subnet`

5. Enter IPv4 CIDR block for yourself. I went with `10.0.10.0/24`. This was chosen because it was in the range of my VPC.

6. Click create subnet to launch.

## Create Route Table

1. Select Route tables on the left hand side of page.

2. Name your route table with your required name. I went with `samuel_tech221_public_RT`.

3. Select the VPC you just created.

4. Click create route table to launch.

### Attach Route Table to Subnet

1. Click subnet association.

2. Click edit subnet association.

3. Select subnet and save.

### Attach Route Table To Internet Gateway

1. Select routes tab.

2. Click edit routes.

3. Click add routes.

4. Select your internet gateway and save changes.

## Launch App Instance From AMI

1. Select your AMI that you have created.

2. Launch a new instance from that AMI.

3. Name the instance. I went with `samuel_tech221_sparta_app`

4. Instance Type is `t2.micro` and Key Pair is `tech221`. This could be different depending on requirements.

5. Under Network Settings

- Tick the box to `Allow SSH traffic from` and select `MY IP`.
- Tick the box to `Allow HTTP traffic from the internet`
- Click the edit button.
- Select the VPC required and this will automatically select the required subnet for you.
- Enable auto-assign public IP.
- Add a new rule with source type `Anywhere` and port range `3000`.

6. Launch your new instance.

## Create Private Subnet

1. Click subnets on left hand side of page.

2. Click create subnet.

3. Select the VPC you just created.

4. Name the subnet with your required name. I went with `samuel_tech221_private_db_subnet` as I want to keep this private for security purpose.

5. Enter IPv4 CIDR block for yourself. I went with `10.0.30.0/24`. This was chosen because it was in the range of my VPC.

6. Click create subnet to launch.

## Create Route Table

1. Select Route tables on the left hand side of page.

2. Name your route table with your required name. I went with `samuel_tech221_private_RT`.

3. Select the VPC you just created.

4. Click create route table to launch.

### Attach Route Table to Subnet

1. Click subnet association.

2. Click edit subnet association.

3. Select your private subnet and click save.

## Launch DB Instance from AMI