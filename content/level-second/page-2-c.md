+++
title = "Create security resources.tf file"
date = 2024-09-01T10:12:06+07:00
weight = 3
chapter = true
pre = "<b>2.3 </b>"
+++
## 3. Create security resources.tf file
In addition to the network_resources.tf file, another essential file in Terraform is the security_resources.tf file. This file is responsible for configuring the security measures for your application, such as defining security groups for both EC2 instances and load balancers.

Within the security_resources.tf file, you may define the configuration of a security group for your EC2 instance. This security group may specify which ports are open and which protocols are allowed for inbound and outbound traffic. For example, the EC2 security group may be configured to allow inbound traffic on ports 80 and 22, which correspond to HTTP and SSH traffic, respectively. This ensures that your EC2 instances can be accessed from the internet for web traffic and from your local machine for SSH access.

The security_resources.tf file may also include the configuration of a security group for your load balancer. This security group specifies the rules that allow traffic to flow from the load balancer to the EC2 instances within the VPC. These rules may include which ports are open and which protocols are allowed for inbound and outbound traffic.

Properly configuring the security_resources.tf file is crucial for ensuring that your application is secure and protected from unauthorized access. By defining security groups for your EC2 instances and load balancers, you can control the flow of traffic both within and outside of the VPC. This helps to minimize the risk of security breaches and ensures that your application is compliant with any relevant regulations or standards.

    # Security Group
    resource "aws_security_group" "two-tier-ec2-sg" {
    name        = "two-tier-ec2-sg"
    description = "Allow traffic from VPC"
    vpc_id      = aws_vpc.two-tier-vpc.id
    depends_on = [
        aws_vpc.two-tier-vpc
    ]

    ingress {
        from_port = "0"
        to_port   = "0"
        protocol  = "-1"
    }
    ingress {
        from_port   = "80"
        to_port     = "80"
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        from_port   = "22"
        to_port     = "22"
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
        from_port   = "0"
        to_port     = "0"
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]
    }

    tags = {
        Name = "two-tier-ec2-sg"
    }
    }

    # Load balancer security group
    resource "aws_security_group" "two-tier-alb-sg" {
    name        = "two-tier-alb-sg"
    description = "load balancer security group"
    vpc_id      = aws_vpc.two-tier-vpc.id
    depends_on = [
        aws_vpc.two-tier-vpc
    ]


    ingress {
        from_port   = "0"
        to_port     = "0"
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
        from_port   = "0"
        to_port     = "0"
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]
    }


    tags = {
        Name = "two-tier-alb-sg"
    }
    }

    # Database tier Security gruop
    resource "aws_security_group" "two-tier-db-sg" {
    name        = "two-tier-db-sg"
    description = "allow traffic from internet"
    vpc_id      = aws_vpc.two-tier-vpc.id

    ingress {
        from_port       = 3306
        to_port         = 3306
        protocol        = "tcp"
        security_groups = [aws_security_group.two-tier-ec2-sg.id]
        cidr_blocks     = ["0.0.0.0/0"]
    }

    ingress {
        from_port       = 22
        to_port         = 22
        protocol        = "tcp"
        security_groups = [aws_security_group.two-tier-ec2-sg.id]
        cidr_blocks     = ["10.0.0.0/16"]
    }

    egress {
        from_port   = 0
        to_port     = 0
        protocol    = "-1"
        cidr_blocks = ["0.0.0.0/0"]
      }
    }
