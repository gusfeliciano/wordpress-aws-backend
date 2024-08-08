# WordPress AWS Backend

This project sets up a scalable and maintainable WordPress backend infrastructure on AWS using CloudFormation.

The goal is to migrate an existing WordPress site to AWS for improved performance, reliability, and scalability.

## Overview

This backend setup includes:

1. EC2 instance for hosting WordPress
2. RDS MySQL database for WordPress data
3. Security groups for controlling access
4. (Future addition) S3 bucket for media storage
5. (Future addition) CloudFront distribution for content delivery

## Prerequisites

- AWS CLI installed and configured with appropriate permissions
- Basic understanding of AWS services (EC2, RDS, CloudFormation)
- Git for version control

## Deployment

1. Clone this repository
2. Update the `wordpress-stack.yaml` file with your desired configurations
3. Deploy the CloudFormation stack using AWS CLI:

```bash
aws cloudformation create-stack --stack-name WordPressStack --template-body file://wordpress-stack.yaml --parameters ParameterKey=DBName,ParameterValue=wordpressdb ParameterKey=DBUser,ParameterValue=admin ParameterKey=DBPassword,ParameterValue=your-secure-password --profile wordpress-project
```

Replace `your-secure-password` with a strong password for your WordPress database.

## Post-Deployment

After successful deployment:

1. Access the EC2 instance and install WordPress
2. Configure WordPress to use the RDS database
3. Install and configure necessary plugins for headless functionality (e.g., WPGraphQL)

## Maintenance

Regular updates and backups are crucial for maintaining a secure and efficient WordPress backend. Consider setting up automated backups and update processes.

## Contributing

Contributions to improve the infrastructure setup are welcome. Please submit pull requests or open issues to discuss potential changes.