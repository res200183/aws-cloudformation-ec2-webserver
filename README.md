# EC2 Web Server with AWS CloudFormation

This project deploys an Nginx web server on Amazon EC2 using AWS CloudFormation and automatically updates the infrastructure through GitHub Actions.

## Resources

The CloudFormation stack creates:

- EC2 Instance
- Security Group
- IAM Role
- IAM Instance Profile
- Nginx web server

## Parameters

- InstanceType - EC2 instance type
- KeyName - EC2 key pair
- AllowedCIDR - CIDR allowed for SSH access

## CI/CD

Every push to the `main` branch automatically triggers GitHub Actions.

The workflow:

1. Checks out the repository.
2. Authenticates to AWS using GitHub OIDC.
3. Assumes an IAM role without storing long-lived AWS access keys in GitHub.
4. Validates the CloudFormation template.
5. Deploys or updates the CloudFormation stack.

Workflow file:

`.github/workflows/deploy.yml`

## Architecture

Developer
   |
   | git push
   v
GitHub Repository
   |
   v
GitHub Actions
   |
   | GitHub OIDC
   v
AWS IAM Role
   |
   v
AWS CloudFormation
   |
   v
EC2 + Security Group + IAM
   |
   v
Nginx Web Server

## Deployment

The infrastructure is deployed automatically after a push to the `main` branch.

Manual deployment is also possible:

aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name ec2-webserver-stack \
  --parameter-overrides \
    InstanceType=t3.micro \
    KeyName=Keyn1 \
    AllowedCIDR=YOUR_IP/32 \
  --capabilities CAPABILITY_NAMED_IAM

## Stack Outputs

CloudFormation returns:

- PublicIP
- WebsiteURL

## Delete Stack

aws cloudformation delete-stack \
  --stack-name ec2-webserver-stack
