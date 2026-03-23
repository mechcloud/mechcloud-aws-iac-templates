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
| **compute** | [public-web-vm-with-eip.md](templates/compute/public-web-vm-with-eip.md) | Public web application VM with an Elastic IP, Internet Gateway, Security Group, and EBS volume |
| | [private-vm-no-public-ip.md](templates/compute/private-vm-no-public-ip.md) | Private EC2 instance with no public IP or Internet Gateway, accessible only within the VPC |
| **networking** | [multi-subnet-vpc.md](templates/networking/multi-subnet-vpc.md) | Two-tier VPC with public frontend subnet and private backend subnet with separate security groups |
| | [vpc-peering.md](templates/networking/vpc-peering.md) | Two VPCs peered together with bidirectional routing and EC2 instances in each |
| | [nat-gateway-private-ec2.md](templates/networking/nat-gateway-private-ec2.md) | Private EC2 with outbound-only internet access via NAT Gateway in a public subnet |
| | [ec2-with-route53.md](templates/networking/ec2-with-route53.md) | EC2 instance with Route 53 hosted zone and A record pointing to an Elastic IP |
| | [vpc-with-vpn-gateway.md](templates/networking/vpc-with-vpn-gateway.md) | VPC with Virtual Private Gateway and site-to-site VPN to on-premises network |
| **load-balancing** | [ec2-with-alb.md](templates/load-balancing/ec2-with-alb.md) | Two EC2 instances behind an Application Load Balancer across multiple availability zones |
| | [autoscaling-group.md](templates/load-balancing/autoscaling-group.md) | Auto Scaling Group with Launch Template and ALB for automatic horizontal scaling |
| **storage** | [ec2-with-s3-bucket.md](templates/storage/ec2-with-s3-bucket.md) | EC2 instance with an S3 bucket configured with versioning and server-side encryption |
| | [ec2-with-efs.md](templates/storage/ec2-with-efs.md) | Two EC2 instances sharing an Elastic File System (EFS) across availability zones |
| **database** | [ec2-with-rds.md](templates/database/ec2-with-rds.md) | EC2 web server in a public subnet with an RDS MySQL database in private subnets |
| | [ec2-with-elasticache.md](templates/database/ec2-with-elasticache.md) | EC2 web server with ElastiCache Redis cluster for in-memory caching |
| | [multi-az-rds.md](templates/database/multi-az-rds.md) | Multi-AZ RDS MySQL with automatic failover and a read replica for scaling reads |
| **cdn** | [cloudfront-s3-static-site.md](templates/cdn/cloudfront-s3-static-site.md) | S3 static website served globally via CloudFront CDN with Origin Access Control |
| **messaging** | [ec2-with-sqs-sns.md](templates/messaging/ec2-with-sqs-sns.md) | EC2 with SQS queue, SNS topic, and dead-letter queue for async messaging |

## Contributing

Contributions are welcome! If you have a useful AWS infrastructure pattern, feel free to submit a pull request. Please follow the existing template structure which includes:

1. A descriptive title and introduction
2. A scenario overview with use case and key features
3. A Mermaid architecture diagram
4. Step-by-step YAML code blocks with explanations
5. A complete unified template at the end

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file.
