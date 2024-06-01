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



# Security Groups

- Here the first security group we’ll create is for the public, internet-facing load balancer. After typing a name and description, let’s add an inbound rule to allow HTTP type traffic for our IP. See below!

   ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/94f6ae60-a09d-4882-9c63-a2ceddf60e2d)

- The second security group for  web tier. After typing a name and description, we need to add an inbound rule that allows HTTP-type traffic from our internet-facing load balancer security group we created in the previous step. This will allow traffic from our public-facing load balancer to hit our instances. After that, we’ll add an additional rule that will allow HTTP-type traffic for our IP. This will allow us to access our instance when we test.


  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/b12dbb93-0c28-4ebd-93a6-5b55491e4dea)

- Now create a Third Internal-load-balancer-sg and add inbond rule type HTTP AND SOURCE WEB tier, that allows HTTP-type traffic from your web tier security group and add another rule for only HTTP
 
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a8b0d5a8-6fdb-48fa-be9d-0d9d0072750b)
 
- The fourth security group we’ll For private-intance-sg add inbond rule type custom TCP and SOURCE Internal-load-balancer-sg, that allows all traffic from your Internal-load-balancer-sg security group and add another rule for only Custom TCP see below :
   
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/36e84085-d94e-45e0-bceb-d671e975c826)

- The fifth security group we’ll configure protects our private database instances. For this security group, we’ll add an inbound rule that will allow traffic from the private instance security group to the MYSQL/Aurora port (3306).
 
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c29b160b-9038-4b67-a1f3-26d43cb11c28)

  

    


  


      

      

 
    



























   


  




  


  

  

  


  


