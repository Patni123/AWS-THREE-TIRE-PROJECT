# AWS-THREE-TIER-PROJECT
project Three Tier Architecture using AWS services.

Step 1: Download Github project in your loacal machine  using below link 
https://github.com/aws-samples/aws-three-tier-web-architecture-workshop.git

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a2628005-f729-40e3-b155-dc239272801f)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/3e692c04-9856-4762-bf6d-7d88f974eb33)


Step 2 :
- Create a IAM Role  AWS-THREE-TIRE-ROLE add permession
- AmazonSSMManagedInstanceCore
- AmazonS3ReadOnlyAccess
-  Create a role 

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/33f37687-01cb-43a7-b676-6751ccc16f17)

  SETP 3:  create the S3 bucket.
  S3 Bucket Nam3- aws-Three-Tire

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/3f37d186-e591-49a7-8501-8e0199443574)


  Step 4:  -	Now Networking and security
-	VPC
-	Subnets
-	Route Tables
-	Internet Gateway
-	NAT gateway
-	Security Groups

  # VPC and Subnets :

  Click on the VPC icon on the AWS Management Console, select ‘Your VPCs’, and click on Create VPC to initiate the creation process.
  Next, select the VPC-only option and fill out the Name tag and CIDR range with the following information:
- Name: Three-tire-application
-	CIDR range: 10.0.0.0/16
-	Accept the default tenancy and create the VPC.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/902b09e8-6062-4adc-be63-7163d1f29b50)

# Subnet Creation:  After creating the VPC, click Subnets on the left side of the dashboard and then click on Create Subnet to create the Subnets.

We will need 6 subnets across two availability zones. This means that three subnets will be in one availability zone and the other three in a second availability zone.

Using the previously created VPC, the 6 subnets with the appropriate configuration settings were successfully created as indicated below.

- Private-1a-db/Private-1b-db 
 (This subnet use for Data-base)


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/e230cab9-0383-4fe2-9a9f-04f5fed23698)

# Internet Connectivity
Internet Gateway

To give our public subnets in our VPC internet access, we need to have an Internet Gateway and attach it to the VPC. On the left-hand side of the VPC dashboard, select Internet Gateway and click on Create internet gateway to create it.

name the Internet Gateway (three-tier-igw)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/8ab413d7-87b4-4da7-b175-ccf82de50f3d)

# NAT Gateway  
In order for our instances in the appLayer private subnet to be able to access the internet they will need to go through a NAT Gateway. For high availability, we’ll deploy one NAT gateway in each of our public subnets.

Let’s navigate to NAT Gateways on the left side of the current dashboard and click Create NAT Gateway.

Fill in the Name, choose one of the public subnets we created in part 2, allocate an Elastic IP, and then click Create NAT gateway.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/0a014eeb-bf02-45bc-97ce-8521852e174a)

Let’s repeat the same procedure for the other public subnet (public-web-subnet-az-1b).


# Routing Configuration :

Now click on route table , after clicking You can see default route table alreday created while you create VPC.
Edit name of route table (Public-ROUTE-TABLE)
Now open Route table At the details page, we’re going to add a route that directs traffic from the VPC to the internet gateway. In other words, for all traffic destined for IPs outside the VPC CDIR range, we need to add an entry that directs it to the internet gateway as a target and save the changes.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c255ee54-a7f2-4183-96b3-ac8962e22fc1)

Now, let’s edit the Explicit Subnet Associations of the route table by navigating to the route table details again. Select Subnet Associations and click Edit subnet associations. Associate two subnet.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/64399c4a-eac0-4906-96c0-532330d6e16c)

- Now Public route tabale work is finished.

- We’re now going to create 2 more route tables, one for each app layer private subnet in each availability zone. These route tables will route app layer traffic destined for outside the VPC to the NAT gateway in the public web subnet respective availability zone, so let’s add the appropriate routes for that. Ready? Let’s go! :)

- Remember we need to create two private route tables for the private app layers 1 and 2 which host the NAT Gateways.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/578a9cd3-bbf1-4e5e-befe-c29bb3b6ee55)

Now Open Private_TABLE -1a and ADD Routes select Nat-getway- AZ-1a and Subnet Associate select Private-1a

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/6c6f2378-1e7a-43a9-91a8-73b1e58db36d)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/ec64e564-65af-49f2-a2b8-27c18177723a)

- Follow same step for Private-table-1b  - open Private_TABLE -1b and ADD Routes select Nat-getway- AZ-1b and Subnet Associate select Private-1b



Step 5: Security Groups

- Here the first security group we’ll create is for the public, internet-facing load balancer. After typing a name and description, let’s add an inbound rule to allow HTTP type traffic for our IP. See below!

   ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/94f6ae60-a09d-4882-9c63-a2ceddf60e2d)

- The second security group for  web tier. After typing a name and description, we need to add an inbound rule that allows HTTP-type traffic from our internet-facing load balancer security group we created in the previous step. This will allow traffic from our public-facing load balancer to hit our instances. After that, we’ll add an additional rule that will allow HTTP-type traffic for our IP. This will allow us to access our instance when we test.


  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/b12dbb93-0c28-4ebd-93a6-5b55491e4dea)

- Now create a Third Internal-load-balancer-sg and add inbond rule type HTTP AND SOURCE WEB tier, that allows HTTP-type traffic from your web tier security group and add another rule for only HTTP
 
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a8b0d5a8-6fdb-48fa-be9d-0d9d0072750b)
 
- The fourth security group we’ll For private-intance-sg add inbond rule type custom TCP and SOURCE Internal-load-balancer-sg, that allows all traffic from your Internal-load-balancer-sg security group and add another rule for only Custom TCP port number 4000 see below :
   
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/36e84085-d94e-45e0-bceb-d671e975c826)

- The fifth security group we’ll configure protects our private database instances. For this security group, we’ll add an inbound rule that will allow traffic from the private instance security group to the MYSQL/Aurora port (3306).
 
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c29b160b-9038-4b67-a1f3-26d43cb11c28)


  Step 6:  Database Deployment
  This section of the workshop will walk us through deploying the database layer of the three-tier architecture.

  Objectives:
  Deploy Database Layer
  Subnet Groups
  Multi-AZ Database

We are going to create database Subnet groups for the architecture. Navigate to the RDS dashboard in the AWS console, search for RDS, and click on Subnet groups on the left-hand side.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f7658176-ff10-4f00-8dc1-3cfa2ae7612d)

Now, we need to add the subnets previously created in each availability zone for the database tier (layer). To ensure consistency, let’s navigate back to the VPC dashboard to verify so that we can select the correct subnet IDs.

Now that we’ve got the subnets IDs verified, let’s select the two subnets to add them and create the Subnet Group as indicated below

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2052947d-9836-4a78-91c7-d885490ebb0b)

# Database Deployment :
Create database. We’re going to select MySQL/Aurora as our database choice. On the same RDS dashboard, navigate to Databases on the upper left-hand side 
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/689f8422-698e-46cf-8d85-cd11f1edb44d)

Under the Templates section, select the Dev/Test since this isn’t being used for production at the moment. Under Settings Database cluster identifier, keep the default name database-1.
We’ll keep the username as ‘admin, set a password to welcome-db’, and then write down the password on a notepad since we’ll be using password authentication to access our database.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2c39027f-8460-47de-b686-6261a9093a4b)

Under the Cluster storage configuration section, we’ll keep Aurora Standard and keep the default option under Instance Configuration

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/76de64a9-327f-465c-953c-c9738c4b72a7)

Next, under Availability and Durability, we’ll keep the option to create an Aurora Replica or reader node in a different availability zone as recommended along with our VPC under Connectivity.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/69e2adbd-a3f4-4047-a0b0-373fd74f7109)


We’ll also choose the subnet group we created earlier, and select no for public access.
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/8c6560ae-72a2-45cd-ad47-725c498311ef)

Now, let’s set the security group we created for the database layer.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f39448cd-475c-45ea-9214-19a4df58f240)

We’ll not make any changes under password authentication because password authentication is always on. Click Create to create the database.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/01498c80-f710-411a-a61b-82f69992acd3)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/394d2fc7-2b40-4266-84fc-1a3297b26da1)


Step 7 :  App Tier Instance Deployment

In this section, we are going to create an EC2 instance for our app layer and make all the essential software configurations so that the app can run correctly. The app layer will consist of a Node.js application running on port 4000. In addition, we will configure our database with some data and tables.

Objectives:
Create App Tier Instance
Configure Software Stack
Configure Database Schema
Test DB connectivity

- App Instance Deployment
Using the EC2 service dashboard, click on Instances on the left-hand side and then Launch Instances to start the process.  Let’s name our app instance ‘appweb’ for the three-tier architecture and select the Amazon Linux 2 AMI for our application and operating system image.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c17c265b-9e40-41ea-af20-42e704bd97d2)

We’ll be using the free tier eligible T.2 micro instance type so let’s select that to proceed. Even though ‘Proceed without a key is (Not recommended)’, we’ll proceed without a key pair for architecture.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f8b11cf0-3c2b-4c02-aac6-c7d6e3d28c7f)

Earlier we created a security group for our private app layer instances, so go ahead and select that along with our default VPC and the ‘private-app-subnet-az-1' under Network settings.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/1b1f7905-5a01-4c8a-ae77-41277a47db54)

We’ll keep the default configuration settings for Configure storage, and Advanced details as-is, but review the Summary, and click Launch instance to create the ‘appLayer’ instance.

The ‘appLayer’ instance is created.

Now, let’s navigate back to the Instance dashboard of the EC2, select the ‘appLayer’ instance, click on Actions, and then Modify IAM role under Security to update the IAM role.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2b29e9f7-0982-40dd-9559-d1ec3e3ba67b)

Select the IAM role we created earlier from the ‘IAM role’ list and click ‘Update IAM role’ to update the role.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c32a6cc8-c436-4c84-a5dc-3409ee3680db)

- Connect to Instance
Let’s navigate to our list of running EC2 Instances by clicking on Instances on the left-hand side of the EC2 dashboard. When the instance state is running, connect to the instance by clicking the checkmark box to the left of the instance and clicking the connect button on the top right corner of the dashboard. Select the Session Manager tab, and click Connect. This will open a new browser tab for you.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/eace494d-2bc8-4627-8eb8-0caba083289e)

When you first connect to your instance like this, you will be logged in as ssm-user which is the default user as indicated below.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/e9c1489a-7228-48c1-b872-3ac1e0c94ff5)

Now, let’s switch to ec2-user using the below command in the same terminal:
sudo -su ec2-user

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/817f7421-68db-4cde-8ac3-e44f2dbf9e85)


Let’s take this moment to make sure that we are able to reach the internet via our NAT gateways. If our network is configured correctly up till this point, we should be able to ping the www.Google.com

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/454d1ae2-2c38-4cc8-93c3-310bfcacc368)

You can stop the ping by pressing ctrl + c.

- Configure Database

  sudo yum install mysql -y

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/d71f060d-84bc-496c-9fa1-ddea43a0be51)


- Now, let’s initiate our DB connection with our Aurora RDS writer endpoint. In the following command, replace the RDS writer endpoint and the username, and then execute it in the browser terminal:

  mysql -h CHANGE-TO-YOUR-RDS-ENDPOINT -u CHANGE-TO-USER-NAME -p

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/ae297f0c-ff73-41cd-aee6-354279bc2260)


  We successfully connected to the database


  Now that we’ve successfully connected to MySQL database, let’s create a database called ‘webappdb’ with the following command using the MySQL CLI:

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/40504295-0290-441b-b49a-9d61c833a5b4)


  Let’s verify that the database was created correctly with the following command:

  SHOW DATABASES;

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/9db71d3a-5e51-42ce-b04d-ed56cdd8538b)

  Now, let’s create a data table by first navigating to the database we just created with the command below:
  USE webappdb;

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/3112c9c1-e8ca-451f-8861-5b0e8405fe01)


Now that we’ve changed to the database (webappdb), let’s create the following transactions table by executing this create table command:

CREATE TABLE IF NOT EXISTS transactions(id INT NOT NULL
AUTO_INCREMENT, amount DECIMAL(10,2), description
VARCHAR(100), PRIMARY KEY(id));

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/d038f465-51ca-4888-b2e2-dcd238970a88)


Let’s verify if the tables were created with the below command:   SHOW TABLES;
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/cf18b8f7-3cd3-4280-b10b-f9e31aca6b0e)

Now that we have the tables created, let’s insert data into the table with the command below for use/testing later:

INSERT INTO transactions (amount,description) VALUES ('400','groceries');

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/e100014f-b223-4457-ab26-d7b8fbbfb632)


Let’s verify that our data was added by executing the following command:

SELECT * FROM transactions;

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f71d8057-c29e-4252-ae1f-6da784fd37f4)


To exit the MySQL client, just type exit and hit enter.








































    


  


      

      

 
    



























   


  




  


  

  

  


  


