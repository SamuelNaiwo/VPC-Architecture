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

5. Click subnet association.

6. Click edit subnet association.

7. Select subnet and save.

### Attach Route Table To Internet Gateway

8. Select routes tab.

9. Click edit routes.

10. Click add routes.

11. Select your internet gateway and save changes.

    ![Alt text](img/Screenshot%202023-04-25%20at%2012.29.13.png)

## Launch App Instance From AMI

1. Select your AMI that you have created for your app.

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

7. Open your terminal/bash window.

8. Change directory into your .ssh folder `cd .ssh`

9. Copy your ssh key from your AWS app instance and paste into terminal: `ssh -i "file.pem" ubuntu@20.55.03.165` and select yes.

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

5. Click subnet association.

6. Click edit subnet association.

7. Select your private subnet and click save.

## Launch DB Instance from AMI

1. Select the AMI you created for your database.

2. Launch a new instance from this AMI.

3. Name the instance. I went with `samuel_tech221_mongo_db`

4. Instance Type is `t2.micro` and Key Pair is `tech221`. This could be different depending on requirements.

5. Under Network Settings

- Tick the box to `Allow SSH traffic from` and select `MY IP`.
- Click the edit button.
- Select the VPC required and select your private subnet you just created. Mine was called `samuel_tech221_private_db_subnet`
- Disbale auto-assign public IP.
- Add a new rule with source type `Anywhere` and port range `27017`.

6. Launch your new instance.

## Connect App and DB Instance

1. Open your terminal connected to your app instance.

2. Use the following command to open your .bashrc folder: `sudo nano .bashrc`

3. Scrolll down to the bottom and delete the environment variable we used before, save the changes and exit.

4. Use the following command to referesh the changes: `source .bashrc`.

5. Open your .bashrc folder again: `sudo nano .bashrc`.

6. Scroll down to the bottom and enter your environment variable. I used in this example `export DB_HOST=mongodb://<private-IP>:27017/posts`

7. Save your changes and exit.

8. Use the following command to referesh the changes: `source .bashrc`.

9. Change directory into app folder. `cd app`

10. Launch the app with `node app.js` or `npm start`

11. Paste your app instance public IPv4 into the browser and your app should load. `63.32.91.173:3000/posts`

    ![Alt text](img/VPC%20NodeApp%20Launch.png)

