# aws-labs-vpc
### OBJECTIVE

Creating an  AWS VPC with Public and Private Subnets. Configuring Route Tables, attaching a NAT Gateway to the Private Subnet , running EC2 Web Server instance on Public Subnet and Private Instance on Private Subnet accessed by a Bastion HOST.


####  1 - Creating a VPC and Subnets
Access AWS Console  https://console.aws.amazon.com after logged in, click on Services menu and select Networking & Content Delivery then click on VPC.

At left panel on **Virtual Private Cloud** menu click on **"Your VPCs"** then clck on **"Create VPC"** button:

_Fill UP your VPC Name, IPv4 CIDR (in this case we are using 10.0.0.0/16) and finish clicking on "Create VPC" button_

![image](https://user-images.githubusercontent.com/48591555/153713506-e8753f3d-e96f-4b57-936d-a3820e7119be.png)

After creating vpc (_vpc-prod1_ in this lab_) , you may need enable DNS Hostnames for this VPC. Select your VPC and click on Actions button and Select _Edit DNS Hostname_:

![image](https://user-images.githubusercontent.com/48591555/153714418-ec080155-299e-4c58-90ee-c6621905a042.png)


_Select "Enable"then "Save Changes"_

![image](https://user-images.githubusercontent.com/48591555/153714365-b9bc6cf9-2046-48f5-91b6-a6da4e7e1e0d.png)


Now we need to create 2 Subnets (_Private and Public_). For this LAB we are going to create Subnets with 2 Availability Zones. 

So We gonna have following Subnets:
1. Public Subnet A - in _us-east-1a_
2. Public Subnet B -  in _us-east-1b_
3. Private Subnet A - in _us-east-1a_
4. Private Subnet B - in _us-east-1b_

Click on Subnets menu then click on **"Create Subnet"** Button with following settings for the Public Subnet:
- Public Subnet A:
_Select your VPC (in this example is the vpc-prod1), giving a name to your Public Subnet (Public-A), select an Availability Zone (us-east-1a) and inform IPv4 CIDR for this Subnet (10.0.0.-/24) then click on Create Subnet_:

![image](https://user-images.githubusercontent.com/48591555/153713871-ef1e8b62-95ff-4826-b48d-bf3af17d19f8.png)

Enable Automatic public ipv4, select Public-A subnet then click on **Actions>Edit subnet Settings:**

![image](https://user-images.githubusercontent.com/48591555/153716042-86c30942-a2bf-47e7-bf50-89cb61f7e536.png)

select to **Ënable auto-assign public IPv4 address**

![image](https://user-images.githubusercontent.com/48591555/153716075-fe5b2c35-a463-41a4-9358-33c954b0109d.png)


Click on Subnets menu then click on **"Create Subnet"** Button with following settings for the Public Subnet:
- Public Subnet B:
_Select your VPC (in this example is the vpc-prod1), giving a name to your Public Subnet (Public-B), select an Availability Zone (us-east-1b) and inform IPv4 CIDR for this Subnet (10.0.1.0/24) then click on Create Subnet_:

![image](https://user-images.githubusercontent.com/48591555/153714130-aad41f59-54a6-4eb1-9335-7fff54f6399e.png)

Enable Automatic public ipv4, select Public-A subnet then click on **Actions>Edit subnet Settings:**

![image](https://user-images.githubusercontent.com/48591555/153716042-86c30942-a2bf-47e7-bf50-89cb61f7e536.png)

select to **Ënable auto-assign public IPv4 address**

![image](https://user-images.githubusercontent.com/48591555/153716080-42673efb-7b04-426b-8e61-a383595b74c6.png)


Click on Subnets menu then click on **"Create Subnet"** Button with following settings for the Private Subnet:
- Private Subnet A:
_Select your VPC (in this example is the vpc-prod1), giving a name to your Private Subnet (Private-A), select an Availability Zone (us-east-1a) and inform IPv4 CIDR for this Subnet (10.0.16.0/20) then click on Create Subnet_:

![image](https://user-images.githubusercontent.com/48591555/153714525-12a5576f-b52c-4947-baaf-f5fdfb3093c9.png)

Click on Subnets menu then click on **"Create Subnet"** Button with following settings for the Private Subnet:
- Private Subnet B:
_Select your VPC (in this example is the vpc-prod1), giving a name to your Private Subnet (Private-B), select an Availability Zone (us-east-1b) and inform IPv4 CIDR for this Subnet (10.0.32.0/20) then click on Create Subnet_:

![image](https://user-images.githubusercontent.com/48591555/153714583-e9d577e1-55e7-4722-9cf7-78a7187bee26.png)


#### 2 - Creating Internet Gateway
To Create an internet Gateway click on Internet Gateways menu then click on Create Internet Gateway button and name your internet gateway. For this LAB internet gateway name is ig-prod-01:

![image](https://user-images.githubusercontent.com/48591555/153714747-2905a5ed-e915-444d-acfe-5bd51a214530.png)

As soon as Internet Gateway has been created, Attach it to _vpc-prod1_:

![image](https://user-images.githubusercontent.com/48591555/153714800-a1373347-b189-4da2-b0c2-32448139d13a.png)

![image](https://user-images.githubusercontent.com/48591555/153714808-debfca98-427c-40d4-a66f-51607586fb79.png)

#### 3 - Creating Route Tables
 - **Public Route Table**
 Click on "_Route Tables_" menu then click on **Create route table** button and Fill following information:
 
 _Name_ : rt-public
 _VPC_ : Select _vpc-prod1_ 
 
 ![image](https://user-images.githubusercontent.com/48591555/153714933-1642b4c6-5960-4204-85a1-9ae727faa958.png)
 
 Click on Subnet Associations then click on "**Edit Subnet associations**" to add Public Subnets selection both (_Public A and B_):
 
 ![image](https://user-images.githubusercontent.com/48591555/153715003-2708787f-d2ad-4c20-bd98-a31a78d6f9d5.png)
 
 Now Click on **Edit Routes** to change Public Route to the internet gateway:
 
 ![image](https://user-images.githubusercontent.com/48591555/153715066-25d862f4-bd7b-4ce8-805a-3069aaef3af7.png)



 - **Private Route Table**
 
 Click on "_Route Tables_" menu then click on **Create route table** button and Fill following information:
 
 _Name_ : rt-private
 _VPC_ : Select _vpc-prod1_ 
 
 ![image](https://user-images.githubusercontent.com/48591555/153715117-46d44b41-5a1a-43be-8258-b8c8421d297a.png)

Click on Subnet Associations then click on "**Edit Subnet associations**" to add Private Subnets selection both (_Private A and B_):

![image](https://user-images.githubusercontent.com/48591555/153715164-7fd0a983-387b-408b-b0d7-5cd8db50a8f1.png)

#### 4 - Creating NAT Gateway
NAT Gateway is a highly available AWS managed service that makes it easy to connect to the Internet from instances within a private subnet in an Amazon Virtual Private Cloud.
To launch a  NAT Gateway, click on **NAT Gateways** in the **VIRTUAL PRIVATE CLOUD** menu then click on **Create NAT gateway** button filling those information:

_Name_: gtw-prod1
_Subnet_: _Public-A_ (NAT Gateways Always should be deployed on a Public Subnet)

![image](https://user-images.githubusercontent.com/48591555/153715372-ee3bb031-2faa-41f2-8ae5-9db6ee1745b4.png)

Allocate an Elastic IP clicking on "**Allocate Elastic IP**" button:

![image](https://user-images.githubusercontent.com/48591555/153715406-c282cffe-377c-481b-8069-6b6c99c7ca53.png)

finish clicking on "** Create NAT Gateway**" button.

Now to enable Private Subnet traffing route to internet trhough NAT Gateway, you may need to add this traffic route to Private Subnet.
Click on Route Tables and select _rt-private_ Route Table then Click on Routes > Edit Routes. Add following Route entry:

![image](https://user-images.githubusercontent.com/48591555/153715528-02123759-2ac6-4869-9fb5-389f4a2622ab.png)


#### 5 - Creating EC2 Instances - WebServer/Bastion Host/ Private Instances

**1.Creating Web Server in a Public Subnet**
To create a EC2 Web Server instance click Search bar in AWS console and type "EC2" then click on EC2.
on left panel find Instances menu then selec Instances and start creating a new instance clicking on **Launch instances** button.
Select first option for _Amazon Linux 2 AMI_ instance:

![image](https://user-images.githubusercontent.com/48591555/153715787-d2070156-1872-4073-9526-c4cb4c141a3e.png)

Select: _t2.micro_ for Instance Type then click on "**Next"Configure Instance details**":

![image](https://user-images.githubusercontent.com/48591555/153715850-2dfad674-579b-4527-bf3a-34c7bc2aadb2.png)

Set following options for Network 

**Network** : **vpc-prod1**
**Subnet** : **Public-A**

![image](https://user-images.githubusercontent.com/48591555/153716157-3a778a89-9650-486f-b27f-a19df49774e4.png)


Add following **"User data**"to build up an apache web server: 

```markdown
#!/bin/bash
# install httpd
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Web Server PROD-01</h1>" > /var/www/html/index.html
```
![image](https://user-images.githubusercontent.com/48591555/153716296-1a23b65d-eb00-4a36-9aa3-e9343f358c29.png)

Click on **Next Add Storage**, there is no need to change then click on **Next Add Tags"**
Click on **Add Tag** and add following Tags:

![image](https://user-images.githubusercontent.com/48591555/153716530-a0d10c28-71fc-4659-9703-685bf864c4f6.png)

Create Following Security Group for the Web-Server
![image](https://user-images.githubusercontent.com/48591555/153716542-79cd120c-a357-4dac-9e87-b70c07f88bda.png)

Create a New-key pair for web-server.

**2.Creating Bastion Host in a Public Subnet**
To create a EC2 Bastion Host instance click Search bar in AWS console and type "EC2" then click on EC2.
on left panel find Instances menu then selec Instances and start creating a new instance clicking on **Launch instances** button.
Select first option for _Amazon Linux 2 AMI_ instance:

![image](https://user-images.githubusercontent.com/48591555/153715787-d2070156-1872-4073-9526-c4cb4c141a3e.png)

Select: _t2.micro_ for Instance Type then click on "**Next"Configure Instance details**":

![image](https://user-images.githubusercontent.com/48591555/153715850-2dfad674-579b-4527-bf3a-34c7bc2aadb2.png)

Set following options for Network 

**Network** : **vpc-prod1**
**Subnet** : **Public-A**

![image](https://user-images.githubusercontent.com/48591555/153716157-3a778a89-9650-486f-b27f-a19df49774e4.png)

Click on **Next Add Storage**, there is no need to change then click on **Next Add Tags"**
Click on **Add Tag** and add following Tags:

![image](https://user-images.githubusercontent.com/48591555/153716750-6a631181-bf4a-4e0b-b7f2-1df03bd3efdc.png)

Create following group for Bastion Host:
![image](https://user-images.githubusercontent.com/48591555/153716817-89a0d02a-9e27-4991-b3b7-b6e2edd4a636.png)

Select same key pair and launch instance

![image](https://user-images.githubusercontent.com/48591555/153716871-e8b15110-0060-4dbb-84ae-8839f5ff2341.png)


**3.Creating Private Instance and acessing from Bastion Host SSH connection.**








