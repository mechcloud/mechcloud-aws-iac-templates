# Deploy a Public Web Application VM with an Elastic IP on AWS

This guide demonstrates how to use MechCloud's stateless Infrastructure-as-Code (IaC) to provision the foundational infrastructure for a public-facing web application or backend server on AWS. 

In this scenario, we will provision a Virtual Machine (EC2 instance) inside an initially private subnet. To make the application accessible to the internet, we will configure an Internet Gateway, update the subnet's route table, and finally attach an Elastic IP (EIP) to the instance. MechCloud's hierarchical syntax makes it easy to visualize and deploy these nested resource relationships without managing complex state files.

## Scenario Overview
**Use Case:** Hosting a web application or API backend that requires a dedicated public IP address and external internet access, while securely restricting SSH access only to your current IP.
**Key MechCloud Features Highlighted:**
- Hierarchical resource nesting (VPC $\rightarrow$ Subnet $\rightarrow$ EC2)
- Dynamic macros (`{{CURRENT_REGION}}`, `{{CURRENT_IP}}`, `{{Image|arm64_ubuntu_24_04}}`)
- Cross-resource referencing (`ref:`)

***

## Step 1: Setting up Networking

The first step is establishing the network boundary. We create a VPC and a subnet. To allow our future VM to communicate with the internet, we provision an Internet Gateway (IGW) and update the route table to direct external traffic (`0.0.0.0/0`) through the IGW. Finally, we define a Security Group to control inbound traffic.

```yaml
resources:
  - type: aws_ec2_vpc
    name: vpc1
    props:
      cidr_block: "10.0.0.0/16"
    resources:
      # 1. Define the Subnet
      - type: aws_ec2_subnet
        name: subnet1
        props:
          cidr_block: "10.0.1.0/24"
          availability_zone: "{{CURRENT_REGION}}a"
          
      # 2. Create Internet Gateway for external access
      - type: aws_ec2_internet_gateway
        name: igw1
        
      # 3. Create a Route Table and associate it with the subnet
      - type: aws_ec2_route_table
        name: public_rt
        resources:
          - type: aws_ec2_route
            name: internet_route
            props:
              destination_cidr_block: "0.0.0.0/0"
              gateway_id: "ref:vpc1/igw1"
      - type: aws_ec2_route_table_association
        name: rta1
        props:
          subnet_id: "ref:vpc1/subnet1"
          route_table_id: "ref:vpc1/public_rt"
          
      # 4. Security Group for application and SSH access
      - type: aws_ec2_security_group
        name: sg1
        props:
          group_name: "mc-sg1"
          group_description: "SG for Web Application Backend"
          security_group_ingress:
            # Restrict SSH to the deployer's current IP
            - ip_protocol: tcp
              from_port: 22
              to_port: 22
              cidr_ip: "{{CURRENT_IP}}/32"
            # Open HTTP traffic for the web app
            - ip_protocol: tcp
              from_port: 80
              to_port: 80
              cidr_ip: "0.0.0.0/0"
```

## Step 2: Provisioning a VM

With the networking foundation in place, we can provision the compute resources. We nest the EC2 instance directly inside the subnet block to automatically inherit network placement. We also provision a standalone `gp3` Elastic Block Store (EBS) volume for persistent application data.

```yaml
# ... (Continuing inside the vpc1/subnet1 resources block) ...
        resources:
          - type: aws_ec2_instance
            name: vm1
            props:
              # Dynamically fetch the latest Ubuntu 24.04 ARM64 AMI
              image_id: "{{Image|arm64_ubuntu_24_04}}"
              instance_type: "t4g.small"
              security_group_ids:
                - "ref:vpc1/sg1"

# ... (Continuing at the root resources level) ...
  - type: aws_ec2_volume
    name: vol1
    props:
      availability_zone: "{{CURRENT_REGION}}a"
      size: 10
      volume_type: "gp3"
      
  # Attach the EBS volume to our VM
  - type: aws_ec2_volume_attachment
    name: vol_attach1
    props:
      device_name: "/dev/sdf"
      instance_id: "ref:vpc1/subnet1/vm1"
      volume_id: "ref:vol1"
```

## Step 3: Provisioning an Elastic IP and Attaching it to the VM

To ensure the web application has a static, reliable entry point that survives instance reboots, we allocate an Elastic IP (EIP). Because we updated the route table to include the public gateway in Step 1, attaching this EIP will immediately make the instance accessible from the internet.

```yaml
# ... (Continuing at the root resources level) ...
  - type: aws_ec2_eip
    name: eip1
    props:
      # Attach the EIP directly to the nested EC2 instance
      instance_id: "ref:vpc1/subnet1/vm1"
```

### Complete Unified Template

For your convenience, here is the complete, unified MechCloud template combining all three steps:

```yaml
resources:
  - type: aws_ec2_vpc
    name: vpc1
    props:
      cidr_block: "10.0.0.0/16"
    resources:
      - type: aws_ec2_internet_gateway
        name: igw1
        
      - type: aws_ec2_route_table
        name: public_rt
        resources:
          - type: aws_ec2_route
            name: internet_route
            props:
              destination_cidr_block: "0.0.0.0/0"
              gateway_id: "ref:vpc1/igw1"
              
      - type: aws_ec2_subnet
        name: subnet1
        props:
          cidr_block: "10.0.1.0/24"
          availability_zone: "{{CURRENT_REGION}}a"
        resources:
          - type: aws_ec2_route_table_association
            name: rta1
            props:
              route_table_id: "ref:vpc1/public_rt"
              
          - type: aws_ec2_instance
            name: vm1
            props:
              image_id: "{{Image|arm64_ubuntu_24_04}}"
              instance_type: "t4g.small"
              security_group_ids:
                - "ref:vpc1/sg1"
                
      - type: aws_ec2_security_group
        name: sg1
        props:
          group_name: "mc-sg1"
          group_description: "SG for Web Application Backend"
          security_group_ingress:
            - ip_protocol: tcp
              from_port: 22
              to_port: 22
              cidr_ip: "{{CURRENT_IP}}/32"
            - ip_protocol: tcp
              from_port: 80
              to_port: 80
              cidr_ip: "0.0.0.0/0"
              
  - type: aws_ec2_volume
    name: vol1
    props:
      availability_zone: "{{CURRENT_REGION}}a"
      size: 10
      volume_type: "gp3"
      
  - type: aws_ec2_volume_attachment
    name: vol_attach1
    props:
      device_name: "/dev/sdf"
      instance_id: "ref:vpc1/subnet1/vm1"
      volume_id: "ref:vol1"

  - type: aws_ec2_eip
    name: eip1
    props:
      instance_id: "ref:vpc1/subnet1/vm1"
```