# 🚀 Deploy NGINX on AWS Fargate (Manual GUI Method)

This guide documents the complete step-by-step process of deploying an **NGINX web server** on **AWS Fargate** using the **GUI (Console)**, with detailed explanations of **why** each step is required.  

---

## 📌 Project Objective
Deploy a lightweight **NGINX web server** using AWS's **serverless Fargate engine**, while understanding the underlying AWS resources.

---

## 🏗️ Architecture Overview (No Load Balancer)

🌐 Internet
↓
🟩 Public Subnet
↓
📦 Fargate Task (Public IP)
↓
🔹 NGINX Container


⚡ *This is a simpler, cost-effective setup for testing (no ALB). In production, an Application Load Balancer is recommended.*  

---

## ⚡ Phase 1: Networking Setup (Foundation)

### ✅ Step 1: Create a VPC
- **Where:** `VPC Console → Your VPCs → Create VPC`
- **Settings:**
  - Name: `NginxOnFargate-VPC`
  - IPv4 CIDR: `10.0.0.0/16`

💡 **Why?** A VPC is your private, isolated section of AWS. All resources (tasks, containers) will run inside it.

---

### ✅ Step 2: Create Public Subnets
- **Where:** `VPC Console → Subnets → Create subnet`
- **Settings (2 subnets in different AZs):**
  - Subnet 1: `NginxOnFargate-PublicSubnet-1` → `ap-south-1a` → `10.0.1.0/24`
  - Subnet 2: `NginxOnFargate-PublicSubnet-2` → `ap-south-1b` → `10.0.2.0/24`

💡 **Why?** Subnets divide your VPC into smaller networks. Public subnets allow direct internet access. Different AZs = high availability.

---

### ✅ Step 3: Create and Attach Internet Gateway (IGW)
- **Where:** `VPC Console → Internet Gateways → Create IGW`
- **Settings:** Name: `NginxOnFargate-IGW`
- **Action:** Attach IGW to `NginxOnFargate-VPC`

💡 **Why?** Acts as the bridge between your VPC and the internet.

---

### ✅ Step 4: Configure Route Table
- **Where:** `VPC Console → Route Tables → Create`
- **Settings:**
  - Name: `NginxOnFargate-Public-RT`
  - Add Route: Destination `0.0.0.0/0` → Target `IGW`
  - Associate with both public subnets

💡 **Why?** Defines where network traffic should go. `0.0.0.0/0` = all traffic goes to the internet.

---

## 🔐 Phase 2: Security

### ✅ Step 5: Create Security Group
- **Where:** `VPC Console → Security Groups → Create`
- **Settings:**
  - Name: `NginxOnFargate-Task-SG`
  - Inbound Rule: Allow `HTTP (80)` from `0.0.0.0/0`

💡 **Why?** Security Groups act as firewalls. This allows public access to NGINX.

---

## ⚙️ Phase 3: ECS Setup

### ✅ Step 6: Create ECS Cluster
- **Where:** `ECS Console → Clusters → Create Cluster`
- **Settings:** 
  - Template: `Networking only`
  - Name: `NginxOnFargate-cluster`

💡 **Why?** ECS Cluster groups and manages tasks/services.

---

### ✅ Step 7: Create Task Definition
- **Where:** `ECS Console → Task Definitions → Create`
- **Settings:**
  - Family: `nginx-service`
  - Launch type: `FARGATE`
  - CPU: `0.25 vCPU` | Memory: `0.5 GB`
  - Task Execution Role: *Create new role*
  - Container:
    - Name: `nginx-container`
    - Image: `nginx:latest`
    - Port: `80` (TCP)
    - Logging: CloudWatch enabled

💡 **Why?** Task Definition = Blueprint that defines the container, resources, and logging.

---

## 🚀 Phase 4: Deployment & Access

### ✅ Step 8: Create ECS Service
- **Where:** `ECS Console → Cluster → Services → Create`
- **Settings:**
  - Launch type: `FARGATE`
  - Task Definition: `nginx-service`
  - Service Name: `nginx-service`
  - Desired Tasks: `1`
  - Networking:
    - VPC: `NginxOnFargate-VPC`
    - Subnets: `PublicSubnet-1 & PublicSubnet-2`
    - Security Group: `NginxOnFargate-Task-SG`
    - Auto-assign Public IP: ✅ Enabled

💡 **Why?** ECS Service ensures that the desired number of containers always run.

---

### ✅ Step 9: Access Application
- **Where:** `ECS Console → Cluster → Tasks → Task ID`
- **Find:** Public IP under **Network**
- **Open in Browser:** `http://<PUBLIC_IP>`

🎉 **Result:** You see the **“Welcome to nginx!”** page.

---

## 📚 Key Learnings
1. 🛡️ **VPC & Networking:** Foundation of secure AWS deployments.  
2. 🔐 **Security Groups:** Control inbound/outbound traffic.  
3. ⚡ **Fargate vs. EC2:** Serverless = no infra management.  
4. 📦 **Task Definition vs. Service:** Blueprint vs. Orchestrator.  
5. 🌍 **Public vs. Private Tasks:** Why a Public IP was necessary.  

---

## ✅ Next Steps
Now that you understand the GUI workflow, you can easily move to **Infrastructure as Code** (CloudFormation/Terraform) to automate this setup.  

