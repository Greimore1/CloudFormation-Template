# Free-Tier AWS Web Infrastructure CloudFormation Template

## Overview

This CloudFormation template provides a free-tier compatible AWS infrastructure setup for a basic web application. It creates a robust, scalable network environment with minimal costs, leveraging AWS Free Tier resources.

### Key Components
- VPC with public subnets
- EC2 web server instance
- S3 static website bucket
- Networking and security configurations

## Prerequisites

### AWS Account Requirements
- Active AWS account
- AWS CLI installed and configured
- Sufficient AWS Free Tier credits
- EC2 Key Pair created in your target region

### Local Setup
1. Install AWS CLI
   ```bash
   # For macOS/Linux
   brew install awscli   # Using Homebrew
   # Or
   pip install awscli   # Using pip

   # For Windows
   # Download and install from AWS website
   ```

2. Configure AWS CLI
   ```bash
   aws configure
   # Enter your AWS Access Key ID
   # Enter your AWS Secret Access Key
   # Enter default region (e.g., us-east-1)
   # Enter output format (json recommended)
   ```

## Deployment Instructions

### 1. Clone the Repository
```bash
git init aws-free-tier-infra
cd aws-free-tier-infra
```

### 2. Customize Template
Before deployment, review and modify `free-tier-cloudformation.yaml`:
- Update AMI ID for your specific region
- Adjust CIDR blocks if needed
- Modify security group rules

### 3. Deploy CloudFormation Stack
```bash
# Basic deployment
aws cloudformation create-stack \
  --stack-name FreeTierWebApp \
  --template-body file://free-tier-cloudformation.yaml \
  --parameters \
    ParameterKey=EnvironmentName,ParameterValue=MyProject \
    ParameterKey=KeyName,ParameterValue=YourKeyPairName \
  --capabilities CAPABILITY_IAM

# Optional: Use AWS CloudFormation console for visual deployment
```

### 4. Verify Deployment
```bash
# Check stack status
aws cloudformation describe-stacks --stack-name FreeTierWebApp

# Retrieve outputs
aws cloudformation describe-stacks \
  --stack-name FreeTierWebApp \
  --query 'Stacks[0].Outputs'
```

## Resource Breakdown

### Networking
- **VPC**: `10.192.0.0/16` CIDR block
- **Subnets**: 
  - Public Subnet 1: `10.192.10.0/24`
  - Public Subnet 2: `10.192.20.0/24`
- **Internet Gateway**: Enables public internet access

### Compute
- **Instance Type**: `t2.micro` (Free Tier eligible)
- **Operating System**: Amazon Linux 2
- **Security Group**: Allows HTTP, HTTPS, SSH access

### Storage
- **S3 Bucket**: Static website hosting
- Public read access configured

## Cost Management

### Free Tier Limits
- EC2 t2.micro: 750 hours/month
- EBS Storage: 30 GB-month
- Data Transfer: 15 GB/month
- S3 Storage: 5 GB

### Best Practices
- Set up AWS Billing Alerts
- Terminate instances when not in use
- Monitor Free Tier usage monthly

## Security Considerations

### Recommendations
- Use strong, unique SSH key
- Implement IAM roles with least privilege
- Enable CloudTrail for audit logging
- Consider adding WAF for web protection

## Troubleshooting

### Common Issues
1. **Deployment Fails**
   - Verify AWS CLI configuration
   - Check region-specific AMI
   - Ensure sufficient permissions

2. **Cannot SSH to Instance**
   - Verify security group rules
   - Check key pair permissions
   - Confirm correct key pair name

## Clean Up

### Delete Stack
```bash
aws cloudformation delete-stack --stack-name FreeTierWebApp
```

