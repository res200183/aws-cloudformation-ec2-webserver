# EC2 Web Server with AWS CloudFormation

This project deploys a simple Nginx web server on Amazon EC2 using AWS CloudFormation.

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
- AllowedCIDR - CIDR allowed to connect through SSH

## Deployment

Deploy the stack:

aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name ec2-webserver-stack \
  --parameter-overrides \
    InstanceType=t3.micro \
    KeyName=Keyn1 \
    AllowedCIDR=YOUR_IP/32 \
  --capabilities CAPABILITY_NAMED_IAM

## Get Stack Outputs

aws cloudformation describe-stacks \
  --stack-name ec2-webserver-stack \
  --query 'Stacks[0].Outputs' \
  --output table

## Delete Stack

aws cloudformation delete-stack \
  --stack-name ec2-webserver-stack

## Result

After deployment, CloudFormation returns:

- PublicIP
- WebsiteURL

Opening WebsiteURL displays:

Web Server deployed with CloudFormation
