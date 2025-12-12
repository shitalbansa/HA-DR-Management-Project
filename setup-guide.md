Setup Guide – High Availability & Disaster Recovery
🏗️ Step 1: Create VPCs in Two Regions
1.Go to VPC Dashboard.

2.Create VPC-1 in Region A (e.g., ap-south-1 – Mumbai).

 o	CIDR block: 10.0.0.0/16
 o	Create 2 public subnets in different AZs.
3.Create VPC-2 in Region B (e.g., us-southeast-1 – Singapore).

o	CIDR block: 10.0.0.0/16
o	Create 2 public subnets in different AZs.
=
⚙️ Step 2: Launch EC2 Instances & Auto Scaling
1.Go to EC2 Dashboard in Region A.

2.Create a Launch Template with:

o	Amazon Linux 2 AMI
o	Instance type: t2.micro 
o	Security group: allow HTTP (80), HTTPS (443), SSH (22).
o	User Data (optional for web app):
o	#!/bin/bash
o	yum install -y httpd
                   echo "Hello from Mumbai Regions" > /var/www/html/index.html
o	systemctl start httpd --now
3.Create an Auto Scaling Group (ASG) using this launch template.

o	Attach ASG to 2 subnets in Region A.
o	Min size = 1, Desired = 2, Max = 4.
4.Repeat steps in Region B, but update User Data: echo "Hello from singapore region" > /var/www/html/index.html
<img width="1907" height="1052" alt="image" src="https://github.com/user-attachments/assets/51f3ce31-fc6d-411f-8dae-febec594fedc" />

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/4be12cc1-1516-40c2-a7fd-749c2a7a9559" />

<img width="1890" height="882" alt="image" src="https://github.com/user-attachments/assets/865e5e45-9ff0-41b5-a137-511218a6a8db" />

<img width="1908" height="767" alt="image" src="https://github.com/user-attachments/assets/0cda1cca-c971-42d8-9b0a-0bf7a4654755" />

🌐 Step 3:
   Setup Target Group and Load Balancer
 1.In Region A, create an Target Group.

  select EC2 instances that you want to target.
 2.In Region A, create an Application Load Balancer (ALB).

o	Attach ALB to the 2 subnets in different AZs.
o	Target Group → attach the ASG.
o	Listener → HTTP (80) forward to Target Group.




