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
 - PowerShell/Terminal (for running the deployment commands)
 - An IAM role named ecsTaskExecutionRole in your AWS account (this is usually created automatically when you first use ECS via the console)

## 🚀 Deployment Steps
 - Save the Template
 - Save the provided YAML as fargate-nginx-no-alb.yml
## 2. Deploy the Stack
 - Run this command:
   ```bash
   aws cloudformation deploy --template-file fargate-nginx-no-alb.yml --stack-name MyNginxStack --capabilities CAPABILITY_IAM
   ```
## 3. Access Your NGINX Server
### After deployment completes (takes 5-10 minutes):

 - Go to the AWS ECS Console
 - Navigate to Clusters → MyNginxStack-cluster
 - Click the "Tasks" tab
 - Click on the task ID to view details
 - Find the Public IP in the Network section
 - Open that IP address in your web browser
 - You should see the default NGINX welcome page!

## 🗑️ Cleanup/Deletion
To avoid ongoing charges, delete the stack when you're done:
```bash
aws cloudformation delete-stack --stack-name MyNginxStack
```
## 📝 Troubleshooting
 - If deployment fails, check CloudFormation events in the AWS Console
 - Ensure the ecsTaskExecutionRole exists in your account
 - Verify your AWS CLI has sufficient permissions to create the resources
