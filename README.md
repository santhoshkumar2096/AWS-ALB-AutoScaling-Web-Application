# AWS-ALB-AutoScaling-Web-Application

# AWS ALB + Auto Scaling Web Application 🚀

## Project Overview

This project demonstrates how to build a highly available and scalable web application architecture using AWS Application Load Balancer (ALB), EC2 instances, Launch Templates, and Auto Scaling Groups.

The goal was to create an architecture where traffic is automatically distributed across multiple EC2 web servers and unhealthy instances can be replaced automatically.

---

# Architecture Diagram

![Architecture](architecture/alb-autoscaling-architecture.png)


---

# AWS Services Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer (ALB)
- Target Groups
- Auto Scaling Groups
- Launch Templates
- Security Groups
- Availability Zones


---


# Implementation Steps

## 1. EC2 Web Servers

Created two EC2 instances running Apache web server.

Server 1:


Hello from Web Server 1


Server 2:


Hello from Web Server 2


Apache was installed using user data:

```bash
yum install httpd -y

systemctl start httpd

systemctl enable httpd
2. Target Group Configuration

Created target group:

web-server-tg

Configuration:

Protocol:

HTTP

Port:

80

Health check:

Path: /
Success Code: 200

Registered:

EC2-1
EC2-2
3. Application Load Balancer

Created:

web-serveralb

Configuration:

Type:

Application Load Balancer

Scheme:

Internet-facing

Listener:

HTTP : 80

Forwarding:

web-server-tg
4. Health Checks

ALB continuously monitors instances.

Healthy state:

EC2-1  Healthy
EC2-2  Healthy

Only healthy instances receive traffic.

5. Launch Template

Created:

web-server-template

Contains:

Amazon Linux AMI
t3.micro instance type
Security group
Apache installation script

User Data:

#!/bin/bash

yum update -y

yum install httpd -y

systemctl start httpd

systemctl enable httpd

echo "<h1>Hello from Auto Scaling Web Server</h1>" > /var/www/html/index.html
6. Auto Scaling Group

Created:

web-server-asg

Configuration:

Minimum:

2 instances

Desired:

2 instances

Maximum:

4 instances

Availability Zones:

ap-south-1a
ap-south-1b

Attached to:

Application Load Balancer
Validation

Verified Auto Scaling instances:

Instance 1
Lifecycle: InService
Health: Healthy


Instance 2
Lifecycle: InService
Health: Healthy
Testing

Accessed application using ALB DNS:

http://<ALB-DNS>

Successfully received website response.

Traffic was distributed across multiple EC2 instances.
