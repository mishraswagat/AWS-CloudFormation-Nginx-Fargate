# NGINX on AWS Fargate (No ALB)
A minimal AWS CloudFormation template to deploy a standalone NGINX web server on AWS Fargate with a public IP address.

## 📋 Overview
This project creates a complete infrastructure stack that deploys an NGINX container running on AWS Fargate. Unlike typical Fargate setups that use an Application Load Balancer (ALB), this template assigns a public IP directly to the Fargate task, making it perfect for simple web servers, demos, or testing environments.

## 🏗️ Architecture
The template creates:

- VPC with public subnets across 2 Availability Zones
- Internet Gateway and routing for public internet access
- Security Group allowing HTTP (port 80) traffic from anywhere
- ECS Cluster and Fargate Task Definition for NGINX
- ECS Service that automatically deploys and maintains the NGINX container

## ⚙️ Prerequisites
 - AWS CLI installed and configured with appropriate permissions
 - PowerShell (for running the deployment commands)
 - An IAM role named ecsTaskExecutionRole in your AWS account (this is usually created automatically when you first use ECS via the console)
