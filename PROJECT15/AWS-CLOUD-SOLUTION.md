# AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY

 ## Project Aim: 
 
 To build a secure infrastructure inside AWS VPC (Virtual Private Cloud) network for "smile communications limited" that uses WordPress CMS for its main business website, and a Tooling Website (https://github.com/nbomasi/tooling) for their DevOps team. As part of the company’s desire for improved security and performance, a decision has been made to use a reverse proxy technology from NGINX to achieve this.

Cost, Security, and Scalability are the major requirements for this project. Hence, implementing the architecture designed below, ensure that infrastructure for both websites, WordPress and Tooling, is resilient to Web Server’s failures, can accomodate to increased traffic and, at the same time, has reasonable cost.

**Project architectural image:**

## A. SETTING UP PROFESSIONAL AWS ACCOUNT AND CREATING A COMPANY'S DOMAIN NAME.

1. Properly configure your AWS account and Organization Unit 

* Create an AWS Master account. (Also known as Root Account)
* Within the Root account, create a sub-account and name it **DevOps**. (You will need another email address to complete this)
* Within the Root account, create an AWS Organization Unit (OU). Name it **Dev**. (We will launch **Dev** resources in there)
* Move the **DevOps** account into the **Dev OU**.
* Login to the newly created AWS account using the new email address.

* Changing username can only be done on CLI not console.
aws iam update-user --user-name Ashley --new-user-name DevOps : To rename user from Ashley to DevOps


2. Create the company domain account (smile-nigeria.tk) and subdomain for the Devops tooling website (tooling.smile-nigeria.tk)

3. Create SSl/TLS certificates

## B. CREATING VIRTUAL PRIVATE CLOUD (VPC): SMILE-VPC

**1. VPC:** smile-vpc created

**2. Subnets:** 6 subnets created: smile-public-subnet-1a; smile-public-subnet-1b; smile-private-subnet-1a; smile-private-subnet-1b; smile-private-subnet-2a; smile-private-subnet-2b

**3. Create route Tables:** smile-RTB-public; smile-RTB-private and associated with the public and private subnets respectively.

**4. Create an internet gateway:** smile-internet-gtw, created and attached to smile-vpc

5. Edit a route in public route table, and associate it with the Internet Gateway. (This is what allows a public subnet to be accisble from the Internet)

**6. Create 3 Elastic IPs:** 34.232.225.171

**7. Creating security group:**
* ALB: Internet facing, so https and http ports be opened.

* **Bastion Servers:** Access to the Bastion servers should be allowed only from workstations(PC thru which to access the bastion) that need to SSH into the bastion servers. Hence, you can use your workstation public IP address. To get this information, simply go to your terminal and type curl ```markdown
www.canhazip.com
```

*** Webservers:** Access to Webservers should only be allowed from the Nginx servers. Since we do not have the servers created yet, just put some dummy records as a place holder, we will update it later.

*** Data Layer:** Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully desinged – only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.

Note: A total of 7 security groups were create: int-ALB; Ext-ALB; Nginx; webservers; Bastion; RDS; EFS security group

## C. CREATING RESOURCES.

**1. EFS:** Create EFS (smile-EFS) in the 2 availability zone , then create access point, that is a mount point for the file system. It will be created for the wwordpress and tooling site

2. Create RDS: To create RDS, we need to create **KMS key and DB subnet group**


3. Creating Bastion Auto Scaling Group:

 1. Create instance and install all the basic packages.

 2. Create an AMI template from the instance

 3. Configure a target group 

 4. Create autoscaling from the target group

3. Creating 3 elastic public IPs: 

> internet gateway for VPC > subnet (public and Private) > route table (public and private)

For Nat gateway, local subnet can talk to the internet while the internet can't communicate back.

For internet gateway the communication is 2 ways.



