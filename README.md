# MechCloud AWS IaC Templates

A curated collection of sample Infrastructure-as-Code (IaC) templates for provisioning AWS resources using [MechCloud's](https://mechcloud.io) stateless IaC engine.

## Overview

This repository contains ready-to-use MechCloud templates that demonstrate how to provision common AWS infrastructure patterns — from simple single-VM deployments to multi-tier architectures with load balancers, databases, and CDN. Each template includes a scenario overview, an architecture diagram, a step-by-step walkthrough, and a complete unified YAML template.

These templates are designed to be used with [MechCloud's Stateless IaC](https://docs.mechcloud.io) feature, which allows you to define and deploy cloud infrastructure using a declarative YAML syntax without managing any state files.

## Key Features

- **Hierarchical resource nesting** — Define parent-child relationships naturally (e.g., VPC → Subnet → EC2)
- **Dynamic macros** — Use built-in macros like `{{CURRENT_REGION}}`, `{{CURRENT_IP}}`, and `{{Image|arm64_ubuntu_24_04}}`
- **Cross-resource referencing** — Reference resources across the hierarchy using `ref:` syntax
- **No state management** — MechCloud handles state automatically; just define your desired infrastructure

## Getting Started

1. Sign up or log in at [MechCloud Portal](https://portal.mechcloud.io)
2. Connect your AWS account
3. Create a new stack and paste any template from this repository
4. Deploy and manage your infrastructure through MechCloud

## Templates

| Folder | Template | Description |
|--------|----------|-------------|
| **cdn** | [cloudfront-s3-static-site.md](templates/cdn/cloudfront-s3-static-site.md) | S3 static website served globally via CloudFront CDN with Origin Access Control |
| | [cloudfront-with-alb-origin.md](templates/cdn/cloudfront-with-alb-origin.md) | CloudFront distribution with ALB origin for dynamic content acceleration |
| **compute** | [public-web-vm-with-eip.md](templates/compute/public-web-vm-with-eip.md) | Public web application VM with an Elastic IP, Internet Gateway, Security Group, and EBS volume |
| | [private-vm-no-public-ip.md](templates/compute/private-vm-no-public-ip.md) | Private EC2 instance with no public IP or Internet Gateway, accessible only within the VPC |
| | [ec2-with-userdata.md](templates/compute/ec2-with-userdata.md) | EC2 instance with user data script to bootstrap Nginx web server |
| | [ec2-placement-group.md](templates/compute/ec2-placement-group.md) | EC2 instances in a placement group for low-latency cluster computing |
| | [ec2-multi-eni.md](templates/compute/ec2-multi-eni.md) | EC2 instance with multiple network interfaces for network segmentation |
| | [ec2-spot-fleet.md](templates/compute/ec2-spot-fleet.md) | EC2 Spot Fleet request for cost-optimized compute workloads |
| | [ec2-dedicated-host.md](templates/compute/ec2-dedicated-host.md) | EC2 instances on a dedicated host for BYOL and compliance |
| | [ec2-scheduled-scaling.md](templates/compute/ec2-scheduled-scaling.md) | EC2 Auto Scaling with scheduled scaling policies for predictable traffic patterns |
| **containers** | [ecs-fargate-service.md](templates/containers/ecs-fargate-service.md) | ECS Fargate service with Application Load Balancer for serverless containers |
| | [ecr-repository.md](templates/containers/ecr-repository.md) | ECR private repository with lifecycle policy and image scanning |
| | [eks-cluster.md](templates/containers/eks-cluster.md) | EKS cluster with managed node group for Kubernetes workloads |
| | [ecs-service-connect.md](templates/containers/ecs-service-connect.md) | ECS Service Connect for service mesh communication between microservices |
| | [app-runner-service.md](templates/containers/app-runner-service.md) | App Runner service for fully managed container deployments |
| **database** | [ec2-with-rds.md](templates/database/ec2-with-rds.md) | EC2 web server in a public subnet with an RDS MySQL database in private subnets |
| | [ec2-with-elasticache.md](templates/database/ec2-with-elasticache.md) | EC2 web server with ElastiCache Redis cluster for in-memory caching |
| | [multi-az-rds.md](templates/database/multi-az-rds.md) | Multi-AZ RDS MySQL with automatic failover and a read replica for scaling reads |
| | [aurora-serverless.md](templates/database/aurora-serverless.md) | Aurora Serverless v2 cluster with automatic capacity scaling |
| | [dynamodb-table.md](templates/database/dynamodb-table.md) | DynamoDB table with global secondary index, auto-scaling, and point-in-time recovery |
| | [elasticache-memcached.md](templates/database/elasticache-memcached.md) | ElastiCache Memcached cluster for distributed caching |
| | [redshift-cluster.md](templates/database/redshift-cluster.md) | Amazon Redshift cluster for data warehousing and analytics |
| | [documentdb-cluster.md](templates/database/documentdb-cluster.md) | Amazon DocumentDB cluster for MongoDB-compatible document workloads |
| **load-balancing** | [ec2-with-alb.md](templates/load-balancing/ec2-with-alb.md) | Two EC2 instances behind an Application Load Balancer across multiple availability zones |
| | [autoscaling-group.md](templates/load-balancing/autoscaling-group.md) | Auto Scaling Group with Launch Template and ALB for automatic horizontal scaling |
| | [nlb-with-ec2.md](templates/load-balancing/nlb-with-ec2.md) | Network Load Balancer with EC2 instances for high-throughput TCP traffic |
| | [alb-path-based-routing.md](templates/load-balancing/alb-path-based-routing.md) | ALB with path-based routing to different target groups |
| **messaging** | [ec2-with-sqs-sns.md](templates/messaging/ec2-with-sqs-sns.md) | EC2 with SQS queue, SNS topic, and dead-letter queue for async messaging |
| | [kinesis-data-stream.md](templates/messaging/kinesis-data-stream.md) | Kinesis Data Stream with Lambda consumer for real-time data processing |
| | [eventbridge-pipes.md](templates/messaging/eventbridge-pipes.md) | EventBridge Pipes for event-driven integration between AWS services |
| **monitoring** | [cloudwatch-alarms-ec2.md](templates/monitoring/cloudwatch-alarms-ec2.md) | CloudWatch alarms with SNS notifications for EC2 instance monitoring |
| | [cloudtrail-logging.md](templates/monitoring/cloudtrail-logging.md) | CloudTrail with S3 and CloudWatch Logs for API audit logging |
| | [config-compliance-rules.md](templates/monitoring/config-compliance-rules.md) | AWS Config rules for automated compliance monitoring |
| **networking** | [multi-subnet-vpc.md](templates/networking/multi-subnet-vpc.md) | Two-tier VPC with public frontend subnet and private backend subnet with separate security groups |
| | [vpc-peering.md](templates/networking/vpc-peering.md) | Two VPCs peered together with bidirectional routing and EC2 instances in each |
| | [nat-gateway-private-ec2.md](templates/networking/nat-gateway-private-ec2.md) | Private EC2 with outbound-only internet access via NAT Gateway in a public subnet |
| | [ec2-with-route53.md](templates/networking/ec2-with-route53.md) | EC2 instance with Route 53 hosted zone and A record pointing to an Elastic IP |
| | [vpc-with-vpn-gateway.md](templates/networking/vpc-with-vpn-gateway.md) | VPC with Virtual Private Gateway and site-to-site VPN to on-premises network |
| | [network-firewall.md](templates/networking/network-firewall.md) | AWS Network Firewall for VPC traffic inspection and filtering |
| | [privatelink-endpoint.md](templates/networking/privatelink-endpoint.md) | VPC PrivateLink endpoints for private access to AWS services |
| | [transit-gateway.md](templates/networking/transit-gateway.md) | Transit Gateway connecting multiple VPCs in a hub-and-spoke architecture |
| | [vpc-flow-logs.md](templates/networking/vpc-flow-logs.md) | VPC Flow Logs shipping to S3 and CloudWatch for network traffic analysis |
| | [global-accelerator.md](templates/networking/global-accelerator.md) | AWS Global Accelerator with ALB endpoints for improved global performance |
| | [route53-failover.md](templates/networking/route53-failover.md) | Route 53 failover routing with health checks for high availability |
| | [vpc-ipv6.md](templates/networking/vpc-ipv6.md) | Dual-stack VPC with IPv6 support for modern networking |
| **security** | [iam-roles-and-policies.md](templates/security/iam-roles-and-policies.md) | IAM roles, policies, and instance profiles for EC2 access |
| | [waf-with-alb.md](templates/security/waf-with-alb.md) | AWS WAF Web ACL attached to an ALB for web application protection |
| | [kms-key-encrypted-resources.md](templates/security/kms-key-encrypted-resources.md) | KMS customer managed key with encrypted S3, EBS, and RDS resources |
| | [secrets-manager.md](templates/security/secrets-manager.md) | Secrets Manager with automatic rotation for credential management |
| | [guardduty-securityhub.md](templates/security/guardduty-securityhub.md) | GuardDuty and Security Hub for centralized threat detection |
| | [vpc-security-baseline.md](templates/security/vpc-security-baseline.md) | VPC security baseline with defense-in-depth network controls |
| **serverless** | [lambda-with-api-gateway.md](templates/serverless/lambda-with-api-gateway.md) | Lambda function fronted by API Gateway HTTP API |
| | [lambda-with-sqs-trigger.md](templates/serverless/lambda-with-sqs-trigger.md) | Lambda function triggered by SQS messages with dead-letter queue |
| | [eventbridge-scheduled-lambda.md](templates/serverless/eventbridge-scheduled-lambda.md) | EventBridge scheduled rule triggering a Lambda function |
| | [step-functions-workflow.md](templates/serverless/step-functions-workflow.md) | Step Functions state machine orchestrating multiple Lambda functions |
| | [lambda-with-dynamodb.md](templates/serverless/lambda-with-dynamodb.md) | Lambda function with DynamoDB table for serverless data processing |
| **storage** | [ec2-with-s3-bucket.md](templates/storage/ec2-with-s3-bucket.md) | EC2 instance with an S3 bucket configured with versioning and server-side encryption |
| | [ec2-with-efs.md](templates/storage/ec2-with-efs.md) | Two EC2 instances sharing an Elastic File System (EFS) across availability zones |
| | [s3-cross-region-replication.md](templates/storage/s3-cross-region-replication.md) | S3 buckets with cross-region replication for disaster recovery |
| | [s3-static-website.md](templates/storage/s3-static-website.md) | S3 static website hosting with custom domain |
| | [fsx-windows.md](templates/storage/fsx-windows.md) | Amazon FSx for Windows File Server for managed SMB file shares |
| | [backup-vault.md](templates/storage/backup-vault.md) | AWS Backup vault with backup plans for automated data protection |

## Contributing

Contributions are welcome! If you have a useful AWS infrastructure pattern, feel free to submit a pull request. Please follow the existing template structure which includes:

1. A descriptive title and introduction
2. A scenario overview with use case and key features
3. A Mermaid architecture diagram
4. Step-by-step YAML code blocks with explanations
5. A complete unified template at the end

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file.
