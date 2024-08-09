# WordPress AWS Backend

This project sets up a scalable and maintainable WordPress backend infrastructure on AWS using CloudFormation. The infrastructure is split into three separate stacks for better modularity and management.

## Overview

This backend setup includes:

1. Networking Stack: VPC, Subnets, Internet Gateway, Route Tables
2. Database Stack: RDS MySQL instance, Security Group for RDS
3. Application Stack: EC2 instance for hosting WordPress, Security Group for EC2

## Prerequisites

- AWS CLI installed and configured with appropriate permissions
- Basic understanding of AWS services (EC2, RDS, VPC, CloudFormation)
- Git for version control

## Deployment Steps

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/wordpress-aws-backend.git
   cd wordpress-aws-backend
   ```

2. Deploy the Networking Stack:
   ```
   aws cloudformation create-stack --stack-name WordPressNetworking --template-body file://networking-stack.yaml --profile wordpress-aws
   ```

3. Deploy the Database Stack:
   ```
   aws cloudformation create-stack --stack-name WordPressDatabase --template-body file://database-stack.yaml --parameters ParameterKey=DBPassword,ParameterValue=YourSecurePassword --profile wordpress-aws
   ```
   Replace `YourSecurePassword` with a secure password for your database.

4. Create an EC2 key pair (if you haven't already):
   ```
   aws ec2 create-key-pair --key-name WordPressKeyPair --query 'KeyMaterial' --output text > WordPressKeyPair.pem --profile wordpress-aws
   ```

5. Deploy the Application Stack:
   ```
   aws cloudformation create-stack --stack-name WordPressApplication --template-body file://application-stack.yaml --parameters ParameterKey=KeyName,ParameterValue=WordPressKeyPair ParameterKey=DBPassword,ParameterValue=YourSecurePassword --capabilities CAPABILITY_IAM --profile wordpress-aws
   ```

6. Retrieve WordPress website URL:
   ```
   aws cloudformation describe-stacks --stack-name WordPressApplication --query 'Stacks[0].Outputs[?OutputKey==`WebsiteURL`].OutputValue' --output text --profile wordpress-aws
   ```

## Post-Deployment

After successful deployment:

1. Access the WordPress site using the URL provided in the previous step.
2. Complete the WordPress installation process through the web interface.

## Security Considerations

- The EC2 instance is publicly accessible via HTTP and SSH. Consider restricting SSH access to your IP address.
- Database credentials are passed as parameters. Consider using AWS Secrets Manager for enhanced security.
- The RDS instance is not publicly accessible and can only be accessed from within the VPC.

## Maintenance

- Regularly update WordPress core, themes, and plugins.
- Monitor AWS resources for performance and costs.
- Implement a backup strategy for both the EC2 instance and RDS database.
- Consider setting up automated patching for the EC2 instance.

## Contributing

Contributions to improve the infrastructure setup are welcome. Please submit pull requests or open issues to discuss potential changes.

## Troubleshooting

If you encounter issues:
1. Check CloudFormation events for each stack for error messages.
2. Verify that all stacks were created successfully.
3. Check EC2 instance logs for any startup script errors.
4. Ensure your AWS CLI profile has necessary permissions.