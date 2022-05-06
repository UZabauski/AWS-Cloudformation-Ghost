# AWS Cloudformation Ghost
Application solution requires high available and secure setup on AWS using DataBase, Filesystem and Compute services. This exercise was created for the purpose of studying AWS services.

## A problem-solving approach
- Use EFS to share data between compute instances;
- Use ASG and Fargate to provide application reliability on compute engines;
- Host application instances in 3 Availability Zones;
- Use RDS capabilities for HA DB (MySQL) solution;
- Use ALB as a single entry point for the application;
- Use Public/Private networks for secure access;
- Use strict security rules to ensure communication between all components of the entire stack;
- Use AWS service(s) to manage secrets;
- Use the EC2 userdata configuration for application provisioning;
- Use IAM roles and instance profiles for controlled access to AWS resources;
- Use AWS CloudWatch to collect metrics and logs;
- Use Tags to mark all resources, e.g. `'Project'='CloudX'`.
As the core application for this task, we will use [Ghost](https://ghost.org/), the leading platform for bloggers.

## How to run the stack
1. Create SSH Key pair:
```
aws ec2 create-key-pair --key-name ghost-ec2-pool
```
2. Store the Database password in a safe place:
```
aws ssm put-parameter --name /ghost/dbpassw --type SecureString --value 'se4c!ymZG5N*'
```
3. Create private ECR repository. Execute `ECR.yml` in Cloudformation.
4. Clone Ghost image from Docker hub to ECR repository:
```
docker pull ghost:latest
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com
docker tag ghost:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/ghost:latest
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/ghost:latest
```
5. Set up all infrastructure. Execute `InfraGhost.yml` in Cloudformation.
6. Set up CloudWatch Dashboard. Execute `CW.yml` in Cloudformation.

## References
1. Ghost configuration: https://ghost.org/docs/config/
2. Ghost Database variables: https://github.com/docker-library/docs/blob/master/ghost/stack.yml