# CloudWatch Monitoring & Auto Scaling Project

This project demonstrates a scalable AWS architecture integrated with:
**CloudWatch monitoring**
**Auto Scaling**
**SNS notifications**

It simulates real-world infrastructure observability, using EC2, ALB, and CloudWatch to monitor performance and respond to load changes automatically.

## Architecture Overview

- **VPC** with 4 subnets (2 public, 2 private) across 2 AZs
- **Application Load Balancer** (ALB) in public subnets (one in each AZ)
- **Auto Scaling Group (ASG)** in private subnets
- **NAT Gateway** to allow private instances to reach the internet
- **Launch Template** installs Apache & CloudWatch Agent
- **CloudWatch** for custom metrics, dashboards, alarms, logs
- **SNS Topic** for email notifications
- **IAM Roles** with least privilege
- **Security Group** for Bastion Host, ALB and ASG

![Architecture Diagram](..\images\CloudWatchProject.jpg)


## Features

- CloudWatch agent for custom metrics (e.g., memory usage)
- Logs pushed to CloudWatch Logs
- Alarms for high/low CPU utilization
- Auto Scaling policy tied to alarm thresholds
- SNS alerts sent via email
- AZ info displayed from each EC2 instance

## Infrastructure Setup

1. **VPC & Networking**
   - 1 VPC: `10.0.0.0/16`
   - 2 Public Subnets (for ALB)
   - 2 Private Subnets (for EC2 instances)
   - Internet Gateway + Route Tables

2. **Security Groups**
   - ALB: HTTP open (port 80)
   - Bastion Host: SSH from myip 
   - ASG(EC2): HTTP/SSH from ALB/   Bastion Host 
   (optional)

3. **Launch Template**
   - Installs Apache & CloudWatch Agent
   - Displays AZ in browser (`curl ALB-DNS`)

4. **Auto Scaling Group**
   - Min: 2, Max: 4 instances
   - Scaling tied to CloudWatch alarms

5. **CloudWatch Configuration**
   - Agent config: `/opt/aws/amazon-cloudwatch-agent`
   - Custom metrics: memory, disk usage
   - Logs pushed to centralized group
   - Alarms for CPU thresholds

6. **SNS Integration**
   - Topic: `cloudwatch-alerts-topic`
   - Email subscription enabled
   - Alert triggered by CPU > 70%


