+++
title = "Create ec2-resources.tf file"
date = 2024-09-01T10:12:06+07:00
weight = 4
chapter = true
pre = "<b>2.4 </b>"
+++

## 4. Create ec2-resources.tf file

The ec2-resources.tf file is a Terraform configuration file that is responsible for defining the resources required to provision EC2 instances in your AWS environment. This file type includes the configuration of two EC2 instances, each with an NGINX web server installed, as well as the allocation of two Elastic IPs for those instances.

The EC2 instances defined in the ec2-resources.tf file may be configured with specific instance types, depending on the requirements of your application. Additionally, the file may include the definition of the AMI used to launch the instances, which specifies the operating system and any additional software or configurations required to run your application.

Once the EC2 instances are defined, the ec2-resources.tf file may allocate two Elastic IPs, which are used to provide static public IP addresses for your EC2 instances. This is useful for scenarios where you need a fixed IP address for your instances, such as for setting up DNS records or for firewall configurations.

Finally, the ec2-resources.tf file may configure the EC2 instances with the NGINX web server, which allows the instances to serve web pages to users. This configuration may include the installation of the NGINX software, as well as any required configurations for serving web pages.

Overall, the ec2-resources.tf file is an essential part of any Terraform project that requires EC2 instances. Properly configuring this file ensures that your instances are provisioned with the correct configurations, software, and network settings and that they can be accessed from the internet with a static IP address.


    # Public subnet EC2 instance 1
    resource "aws_instance" "two-tier-web-server-1" {
    ami                         = "ami-0d07675d294f17973"
    instance_type               = "t2.micro"
    subnet_id                   = aws_subnet.two-tier-pub-sub-1.id
    associate_public_ip_address = true
    vpc_security_group_ids      = [aws_security_group.two-tier-ec2-sg.id]
    key_name                    = "kevinaws-ecs"
    tags = {
        Name = "two-tier-web-server-1"
    }
    user_data = <<-EOF
                #!/bin/bash
                sudo yum update -y
                sudo amazon-linux-extras install nginx1 -y || sudo yum install nginx -y
                sudo systemctl enable nginx
                sudo systemctl start nginx
                EOF

    }

    resource "aws_instance" "two-tier-web-server-2" {
    ami                         = "ami-0d07675d294f17973"
    instance_type               = "t2.micro"
    subnet_id                   = aws_subnet.two-tier-pub-sub-2.id
    associate_public_ip_address = true
    vpc_security_group_ids      = [aws_security_group.two-tier-ec2-sg.id]
    key_name                    = "kevinaws-ecs"
    tags = {
        Name = "two-tier-web-server-2"
    }

    user_data = <<-EOF
                #!/bin/bash
                sudo yum update -y
                sudo amazon-linux-extras install nginx1 -y || sudo yum install nginx -y
                sudo systemctl enable nginx
                sudo systemctl start nginx
                EOF
    }


    # EIP
    resource "aws_eip" "two-tier-web-server-1-eip" {
        domain = "vpc"
    depends_on = [aws_internet_gateway.two-tier-igw]
    instance = aws_instance.two-tier-web-server-1.id
    tags = {
        Name = "two-tier-eip-1"
    }
    }

    resource "aws_eip" "two-tier-web-server-2-eip" {
        domain = "vpc"
    depends_on = [aws_internet_gateway.two-tier-igw]
    instance = aws_instance.two-tier-web-server-2.id
    tags = {
        Name = "two-tier-eip-2"
      }
    }
