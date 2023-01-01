# AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY

 ## Project Aim: 
 
To build a secure infrastructure inside AWS VPC (Virtual Private Cloud) network for "smile communications limited" that uses WordPress CMS for its main business website, and a Tooling Website (https://github.com/nbomasi/tooling) for their DevOps team. As part of the company’s desire for improved security and performance, a decision has been made to use a reverse proxy technology from NGINX to achieve this.

Cost, Security, and Scalability are the major requirements for this project. Hence, implementing the architecture designed below, ensure that infrastructure for both websites, WordPress and Tooling, is resilient to Web Server’s failures, can accomodate to increased traffic and, at the same time, has reasonable cost.

**Project architectural image:**
![project15-project-design](https://user-images.githubusercontent.com/65962095/210179126-88db5025-0780-458f-9959-0d66f275b66b.png)

======================================================================================================================================================================
## A. SETTING UP PROFESSIONAL AWS ACCOUNT AND CREATING A COMPANY'S DOMAIN NAME.

1. Properly configure your AWS account and Organization Unit 

* Create an AWS Master account. (Also known as Root Account)
* Within the Root account, create a sub-account and name it **DevOps**. (You will need another email address to complete this)
* Within the Root account, create an AWS Organization Unit (OU). Name it **Dev**. (We will launch **Dev** resources in there)
* Move the **DevOps** account into the **Dev OU**.
* Login to the newly created AWS account using the new email address.

* Changing username can only be done on CLI not console.
aws iam update-user --user-name Ashley --new-user-name DevOps : To rename user from Ashley to DevOps

2. Create the company domain account (smile-nigeria.tk), go to aws route53 to create a hosted zone and copy route traffiic to the name server of the organization where you registered the domain name. Create simple dns record agains both wordpress and tooling site that point to the external load balancer.

3. Create SSl/TLS certificates

=====================================================================================================================================================================
## B. CREATING VIRTUAL PRIVATE CLOUD (VPC): SMILE-VPC

**1. VPC:** smile-vpc created

**2. Subnets:** 6 subnets created: smile-public-subnet-1a; smile-public-subnet-1b; smile-private-subnet-1a; smile-private-subnet-1b; smile-private-subnet-2a; smile-private-subnet-2b

**3. Create route Tables:** smile-RTB-public; smile-RTB-private and associated with the public and private subnets respectively.

**4. Create an internet gateway:** smile-internet-gtw, created and attached to smile-vpc

5. Edit a route in public route table, and associate it with the Internet Gateway. (This is what allows a public subnet to be accisble from the Internet)

**6. Create 3 Elastic IPs:** 34.232.225.171

**7. Creating security group:**

* **ALB:** Internet facing, so https and http ports be opened.

* **Bastion Servers:** Access to the Bastion servers should be allowed only from workstations(PC thru which to access the bastion) that need to SSH into the bastion servers. Hence, you can use your workstation public IP address. To get this information, simply go to your terminal and type 

curl www.canhazip.com

*** Webservers:** Access to Webservers should only be allowed from the Nginx servers. Since we do not have the servers created yet, just put some dummy records as a place holder, we will update it later.

*** Data Layer:** Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully desinged – only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.

Note: A total of 7 security groups were create: int-ALB; Ext-ALB; Nginx; webservers; Bastion; RDS; EFS security group

==================================================================================================================================================
## C. CREATING RESOURCES.

**1. EFS:** Create EFS (smile-EFS) in the 2 availability zone , then create access point, that is a mount point for the file system. It will be created for the wwordpress and tooling site

**2. Create RDS:** To create RDS, we need to create **KMS key and DB subnet group**


**3. Creating Bastion Auto Scaling Group; reverse-proxy nginx server Auto Scaling Group; webserver Auto Scaling Group**

 1. Create instance and install all the basic packages.

 2. Create an AMI template from the instance

 3. Configure a target group 

 4. Create autoscaling from the target group

 5. Create 3 centOs instances (Bastion, nginx and web servers)

 **The above 4 steps will be repeated for reverse-proxy nginx server and webserver**

**Notes:**

Package installation steps are in the file name **"installation.conf"**

* Target group is meant for **servers** that are to be attached to **LBs**

* All the necessary package unique to each server are found in respective **userdata.md file**.

* reverse proxy configuration files are found in **reverse.conf**

**Note:** after creating the bastion autoscale group, login into aws RDS remotely through **bastion **to create **tooling-db** and **wordpressdb**

**4. Creating External and Internal Load balancers(LB)**: Before creating the LBs, all the servers that are connected to the LB are: **nginx, wordpress and tooling server** 

* **Target groups** are created for each of them which is then attached when creating the LBs, Its through this target group that you monitor the health of the servers.

wordpress image
![project15-wordpress-page](https://user-images.githubusercontent.com/65962095/210179040-aea76a6d-601c-41bd-bfa1-3051fb5d4dc5.png)


tooling website image:
![Tooling-image](https://user-images.githubusercontent.com/65962095/210179056-8bcc9ccf-c125-4da4-93b1-2d5a36c065fd.png)



 



