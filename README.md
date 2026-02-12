Hello World Web Application - Terraform Infrastructure
A production-ready "Hello World" web application deployed on AWS using Terraform, featuring high availability, proper security, and automated deployment.
 Architecture Overview
This project creates a complete AWS infrastructure with:

VPC: Custom VPC with public and private subnets
High Availability: Resources deployed across 2 availability zones
Web Servers: 2 EC2 instances (t2.micro) running Nginx in private subnets
Load Balancing: Application Load Balancer distributing traffic
Security: Properly configured security groups following least privilege
Networking: Internet Gateway and NAT Gateway for secure internet access

Architecture Diagram
                            Internet
                               │
                               ▼
                      ┌────────────────┐
                      │ Internet Gateway│
                      └────────┬───────┘
                               │
            ┌──────────────────┴──────────────────┐
            │         VPC (10.0.0.0/16)           │
            │                                      │
            │  ┌────────────────────────────────┐ │
            │  │   Application Load Balancer    │ │
            │  │     (Public Subnets)           │ │
            │  └──────────┬──────────┬──────────┘ │
            │             │          │             │
            │  ┌──────────▼─────┐ ┌─▼──────────┐  │
            │  │ Public Subnet  │ │Public Subnet│  │
            │  │  10.0.1.0/24   │ │ 10.0.2.0/24 │  │
            │  │  us-east-1a    │ │ us-east-1b  │  │
            │  │ ┌────────────┐ │ │             │  │
            │  │ │NAT Gateway │ │ │             │  │
            │  │ └──────┬─────┘ │ │             │  │
            │  └────────┼───────┘ └─────────────┘  │
            │           │                           │
            │  ─────────┼────────────────────       │
            │           │                           │
            │  ┌────────▼────────┐ ┌─────────────┐ │
            │  │ Private Subnet  │ │Private Subnet│ │
            │  │  10.0.11.0/24   │ │10.0.12.0/24  │ │
            │  │  us-east-1a     │ │ us-east-1b   │ │
            │  │  ┌───────────┐  │ │┌───────────┐ │ │
            │  │  │EC2 + Nginx│  │ ││EC2 + Nginx│ │ │
            │  │  │ (t2.micro)│  │ ││(t2.micro) │ │ │
            │  │  └───────────┘  │ │└───────────┘ │ │
            │  └─────────────────┘ └─────────────┘ │
            └──────────────────────────────────────┘
📋 Prerequisites
Before you begin, ensure you have:

Terraform (v1.0 or higher) - Install Guide
AWS CLI (v2.x) - Install Guide
AWS Account with appropriate permissions
Git (for version control)

AWS Permissions Required
Your IAM user needs permissions for:

EC2 (VPC, Subnets, Security Groups, Instances)
Elastic Load Balancing (ALB, Target Groups)
IAM (if creating instance profiles)

 Quick Start
1. Clone and Setup
bash# Clone the repository (or download the files)
git clone <your-repo-url>
cd terraform-hello-world

# Initialize Git (if not cloned)
git init
2. Configure AWS Credentials
bash# Configure AWS CLI
aws configure

# Enter your:
# - AWS Access Key ID
# - AWS Secret Access Key
# - Default region (us-east-1)
# - Output format (json)

# Verify configuration
aws sts get-caller-identity
3. Configure Variables (Optional)
bash# Copy the example file
cp terraform.tfvars.example terraform.tfvars

# Edit with your values (optional)
nano terraform.tfvars
4. Initialize Terraform
bashterraform init
Expected output:
Initializing the backend...
Initializing provider plugins...
Terraform has been successfully initialized!
5. Review the Plan
bashterraform plan
Review the resources that will be created (~28-30 resources).
6. Deploy Infrastructure
bashterraform apply

# Review the changes
# Type 'yes' when prompted
Deployment takes approximately 5-7 minutes.
7. Access Your Application
bash# Get the ALB URL
terraform output alb_url

# Test with curl
curl $(terraform output -raw alb_url)

# Or open in browser
# Copy the URL from terraform output alb_url
You should see the "Hello World" page with instance details!
