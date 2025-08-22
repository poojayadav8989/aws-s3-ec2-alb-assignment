----------------------------------------:steps that i have followed:-----------------------------------------


Part A – S3 Setup

Create an S3 bucket (block all public access, enable KMS encryption if asked).

Upload files/screenshots into the bucket.

Enable CloudWatch logging/metrics for S3 to view upload logs.

Part B – EC2 + ALB

Step 1 – Security Groups

alb-sg: Allow HTTP (80) from 0.0.0.0/0.

ec2-sg: Allow HTTP (80) only from alb-sg, SSH (22) from my IP.

Step 2 – Launch 2 EC2 Instances

Name: web-1, web-2

AMI: Amazon Linux 2023, type: t2.micro

VPC: default, different subnets

Security group: ec2-sg

User Data installs Nginx and sets index page.

Step 3 – Create Target Group

Type: Instances, Protocol: HTTP, Port: 80, VPC: default.

Health check path /.

Register both EC2 instances → ensure status Healthy.

Step 4 – Create Application Load Balancer (ALB)

Internet-facing, HTTP (80), VPC: default, subnets: at least 2 AZs.

Security group: alb-sg.

Attach target group.

Step 5 – Test

Copy ALB DNS name → open in browser → traffic alternates between web-1 and web-2.
