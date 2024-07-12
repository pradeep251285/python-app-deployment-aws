# AWS-PYTHON application deployment in AWS

**Deploying web Application using PYTHON**

# Project Description
High-Availability Python Application Deployment on AWS with Automated Scaling and Management
This project involves deploying a high-availability Pyhton application on AWS using Elastic compute cloud, with an external Amazon RDS database. The infrastructure setup ensures reduced deployment time, higher availability, and scalability, leveraging various AWS services and best practices.

# Key Feature of this Project

1.**Employee Information Input:**
- A user-friendly interface for employees to input their details, including name, location, age, and technology.

2.**Photo Upload:** 
- Functionality for employees to upload their photos, which are stored in an Amazon S3 bucket.

3.**Employee Information Retrieval:**
- Allows users to retrieve stored employee information using a unique employee ID.

4.**Scalable and High Availability:**
- Utilizes Amazon EC2 for deploying the PYTHON application, ensuring scalability and high availability.

5.**Secure Data Storage:**
- Employee details and image metadata are securely stored in an Amazon RDS (MySQL) database

6.**AWS Lambda Integration:**
- A Lambda function is triggered when an object is uploaded to the S3 bucket, allowing for additional processing or notifications.

7.**SNS Notifications:**
- Simple Notification Service (SNS) topic to notify subscribed users via email when an object is uploaded to the S3 bucket.

8.**Custom VPC Configuration:**
- A custom Virtual Private Cloud (VPC) setup with public and private subnets for enhanced security and network segregation.

9.**Auto Scaling:**
- The application is configured with an Auto Scaling Group to handle varying loads and ensure optimal performance.

# Prerequistes
1.Knowledge about AWS services like EC2,LAMBDA,RDS,S3 ETC.....

# Project Architecture

![project architecture](https://github.com/Nikhil1422003/AWS-Infrastructure/assets/155822950/e93c8f61-608e-4362-90d9-84989315bb7a)




# Steps to create this infrastructure
 go to the aws management console.


**1. Set Up VPC, Subnets, and Security Groups**

#### Create a VPC:

- Open the VPC Dashboard.
- Click on "Create VPC".
- Select "VPC only" and configure:
- Name tag: MyVPC
- IPv4 CIDR block: 10.0.0.0/16
- Tenancy: Default
- Click "Create VPC".
#### Create Subnets:

- Open the Subnets section.
- Click on "Create subnet".
- Select your VPC (MyVPC).
- Create one public subnet:
- Name tag: PublicSubnet
- IPv4 CIDR block: 10.0.1.0/24
- Create two private subnets:
- Name tag: PrivateSubnet1
- IPv4 CIDR block: 10.0.2.0/24
- Name tag: PrivateSubnet2
- IPv4 CIDR block: 10.0.3.0/24
#### Create an Internet Gateway:

- Open the Internet Gateways section.
- Click on "Create internet gateway".
- Name tag: MyInternetGateway.
- Attach it to your VPC (MyVPC).
#### Create a Route Table for the Public Subnet:

- Open the Route Tables section.
- Click on "Create route table".
- Name tag: PublicRouteTable.
- VPC: MyVPC.
- Edit routes, add route:
- Destination: 0.0.0.0/0
- Target: Your Internet Gateway (MyInternetGateway)
- Edit subnet associations, add PublicSubnet.
- Create a Route Table for the Private Subnets:

- Repeat the above steps for the private subnets, but do not add an internet route.

**2.Create instance**
- launch ec-2 instance
- choose vpc that we created previosly
- connect public instance & update the machine
- Open command prompt and enter ```cd downloads ``` then enter the command
- ``` scp -i "keypair-name" "keypair-name"ubuntu@(ip of the private instance):/home/ubuntu ```
- After entering the command use ``` cat <keypair-name> ```and copy the key pair
- Then open the private instance connect through SSH command
- After connecting the terminal update the machine by command ``` sudo apt-get update ```
- once above steps are completed enter the following commands
- ``` sudo apt-get install python3 -y ```
- ``` sudo apt-get install python3-boto3 -y ```
- ``` sudo apt-get install python3-flask -y ```
- ``` sudo apt-get install mysql-server -y ```
- ``` sudo apt-get install python3-pymysql -y ```


**3. Create Load Balancer and Auto Scaling Group**
#### Create a Security Group for Load Balancer:

- Open the Security Groups section.
- Click on "Create security group".
- Name: LB-SG
- VPC: MyVPC
- Add inbound rules for HTTP (80) and HTTPS (443).
#### Create the Load Balancer:

- Open the EC2 Dashboard, go to Load Balancers.
- Click on "Create Load Balancer".
- Choose "Application Load Balancer".
- Name: MyALB
- Scheme: internet-facing
- Network Mapping: Select your VPC and PublicSubnet.
- Security Groups: Select LB-SG.
- Listeners: Add HTTP (80) and HTTPS (443) listeners.
- Configure routing, target group: MyTargetGroup.
 #### Create a Security Group for EC2 Instances:

- Repeat the security group creation steps, name it EC2-SG.
- Add inbound rules for HTTP (80), HTTPS (443), and allow traffic from LB-SG.
#### Create an Auto Scaling Group:

- Open the EC2 Dashboard, go to Auto Scaling Groups.
- Click on "Create Auto Scaling group".
- Configure the launch template with:
- AMI: Amazon Linux 2
- Instance type: t2.micro
- Security Group: EC2-SG
- Key pair: Create or select an existing one
- Configure the Auto Scaling group with:
- VPC: MyVPC
- Subnets: Select both PrivateSubnet1 and PrivateSubnet2
- Target Group: MyTargetGroup
- Configure scaling policies.

**4. Create RDS DB Instance and DynamoDB Table**
#### Create an RDS Security Group:

- Repeat the security group creation steps, name it RDS-SG.
- Add inbound rules to allow traffic from EC2-SG.
#### Create the RDS Instance:

- Open the RDS Dashboard.
- Click on "Create database".
- Choose standard create, MySQL, Free tier.
- Configure settings:
- DB instance identifier: mydbinstance
- Master username: admin
- Master password: password
- Instance configuration: db.t2.micro.
- Storage: 20 GB.
- Network: VPC: MyVPC, Subnets: Select PrivateSubnet1 and PrivateSubnet2.
- Security Groups: Select RDS-SG.

### Clone the repository 
Once the above-mentioned steps are completed, navigate to ec2 terminal and clone the given repository:
```
https://github.com/Nikhil1422003/aws-code-main.git
```
#### Create a DynamoDB Table:

- Open the DynamoDB Dashboard.
- Click on "Create table".
- Table name: MyDynamoTable.
- Partition key: ID (String).

#### Connect RDS database in ec2

Once the above-mentioned steps are completed, connect RDS database in ec2:
```
mysql -h endpoint of RDS -u (username) -p (password)
``` 
#### create database in ec2 terminal with below commands

Once the above-mentioned steps are completed, apply the following commands to create the database:
```
create database project;
show databases;
use project;
show tables;

```

once the steps completed go to repo that cloned previously and move to the directory named as **aws-code-main**
and configure the file i.e **config.py** as per our requirements like database name etc.....

#### 5. Create S3 Bucket
- Create an S3 Bucket:
- Open the S3 Dashboard.
- Click on "Create bucket".
- Name: mybucket.
- Region: us-east-1.
- Leave default settings.


#### 6. Create Instance Profile

#### Create an IAM Role:

- Open the IAM Dashboard.
- Click on "Roles".
- Click on "Create role".
- Select "AWS service" and EC2.
- Attach policies: AmazonRDSFullAccess, AmazonDynamoDBFullAccess, AmazonS3FullAccess.
- Name: EC2InstanceProfileRole.
#### Create an Instance Profile:
  
- Click on "Instance profiles".
- Click on "Create instance profile".
- Name: EC2InstanceProfile.
- Attach the role EC2InstanceProfileRole.

#### 7. Create Lambda Function and SNS Topic

#### Create an S3 Event Notification:

- Open the S3 Dashboard, select your bucket (mybucket).
- Click on "Properties".
- Scroll to "Event notifications".
- Click on "Create event notification".
- Configure to trigger on "All object create events".
- Send to Lambda function (create new).
#### Create a Lambda Function:

- Open the Lambda Dashboard.
- click on "Create function".
- Name: S3EventLambda.
- Runtime: Python 3.x.
- Role: Create a new role with basic Lambda permissions.
- Write the function code to handle the S3 event.
#### Create an SNS Topic:

- Open the SNS Dashboard.
- Click on "Create topic".
- Name: MySNSTopic.
#### Create a subscription to your email address.
- Configure Lambda to Publish to SNS:

- Update the Lambda function to publish messages to MySNSTopic.

Once the above-mentioned steps are completed, apply the following command to run the application:
```
sudo python3 EmpApp.py
```


- once  all the progress completed copy the loadbalancer DNS and paste in the browser we will get output of the image given below

