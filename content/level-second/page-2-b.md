+++
title = "Create network resources.tf file"
date = 2024-09-01T10:12:06+07:00
weight = 2
chapter = true
pre = "<b>2.2 </b>"
+++
## 2.Create network resources.tf file
When working with Terraform, one of the most important files you’ll need to create is the network_resources.tf file. This file typically contains the configuration for the network infrastructure that your application will require to function properly. This can include the creation of a Virtual Private Cloud (VPC), which serves as an isolated network environment for your application, as well as the configuration of subnets, an internet gateway, route tables, and load balancers.

Within the network_resources.tf file, you may define two public subnets and two private subnets, each with its own unique configurations. The public subnets may be configured to allow inbound traffic from the internet, while the private subnets may be configured to only allow inbound traffic from within the VPC.

Additionally, the network_resources.tf file may include the configuration of an internet gateway, which serves as a gateway to allow traffic to flow between the VPC and the internet. This file may also include the configuration of route tables and their associations, which are necessary for routing traffic between the subnets within the VPC.

A load balancer may also be included in the network_resources.tf file. This load balancer can be configured with a listener and a target group, which is used to direct traffic to specific instances within the VPC. The file may also include the attachment of a target group to the load balancer, which specifies which instances to direct traffic to.

Finally, the network_resources.tf file may include the configuration of an RDS database subnet group. This group specifies which subnets are used for your RDS instances and can be used to ensure that your RDS instances are properly isolated from other resources within the VPC.

Overall, the network_resources.tf file is an essential part of any Terraform project that requires network infrastructure. Properly configuring this file ensures that your application can communicate with other resources both within and outside of the VPC.

    # Create a vpc
    resource "aws_vpc" "two-tier-vpc" {
    cidr_block = "10.0.0.0/16"
    tags = {
        Name = "two-tier-vpc"
    }
    }

    # Create two public subnets in different AZs
    resource "aws_subnet" "two-tier-pub-sub-1" {
    vpc_id     = aws_vpc.two-tier-vpc.id
    cidr_block = "10.0.0.0/18"
    availability_zone = "ap-southeast-1a"
    tags = {
        Name = "two-tier-pub-sub-1"
    }
    }

    resource "aws_subnet" "two-tier-pub-sub-2" {
    vpc_id     = aws_vpc.two-tier-vpc.id
    cidr_block = "10.0.64.0/18"
    availability_zone = "ap-southeast-1b"
    tags = {
        Name = "two-tier-pub-sub-2"
    }
    }

    # create two private subnets in different AZs
    resource "aws_subnet" "two-tier-prv-sub-1" {
    vpc_id     = aws_vpc.two-tier-vpc.id
    cidr_block = "10.0.128.0/18"
    availability_zone = "ap-southeast-1a"
    tags = {
        Name = "two-tier-prv-sub-1"
    }
    }

    resource "aws_subnet" "two-tier-prv-sub-2" {
    vpc_id     = aws_vpc.two-tier-vpc.id
    cidr_block = "10.0.192.0/18"
    availability_zone = "ap-southeast-1b"
    tags = {
        Name = "two-tier-prv-sub-2"
    }
    }

    # create internet gateway
    resource "aws_internet_gateway" "two-tier-igw" {
    vpc_id = aws_vpc.two-tier-vpc.id
    tags = {
        Name = "two-tier-igw"
    }
    }

    # Create Route table 
    resource "aws_route_table" "two-tier-rt" {
    vpc_id = aws_vpc.two-tier-vpc.id

    route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.two-tier-igw.id
    }

    tags = {
        Name = "two-tier-rt"
    }
    }

    # Route table association
    resource "aws_route_table_association" "two-tier-rt-1" {
    subnet_id      = aws_subnet.two-tier-pub-sub-1.id
    route_table_id = aws_route_table.two-tier-rt.id
    }

    resource "aws_route_table_association" "two-tier-rt-2" {
    subnet_id      = aws_subnet.two-tier-pub-sub-2.id
    route_table_id = aws_route_table.two-tier-rt.id
    }

    # Create load balancer
    resource "aws_lb" "two-tier-alb" {
    name               = "two-tier-alb"
    internal           = false
    load_balancer_type = "application"
    security_groups    = [aws_security_group.two-tier-ec2-sg.id]
    subnets            = [aws_subnet.two-tier-pub-sub-1.id, aws_subnet.two-tier-pub-sub-2.id]

    tags = {
        Environment = "two-tier-alb"
    }
    }

    # Create target group
    resource "aws_lb_target_group" "two-tier-lb-tg" {
    name     = "two-tier-lb-tg"
    port     = 80
    protocol = "HTTP"
    vpc_id   = aws_vpc.two-tier-vpc.id
    }

    # Create Load Balancer Listener
    resource "aws_lb_listener" "two-tier-lb-listener" {
    load_balancer_arn = aws_lb.two-tier-alb.arn
    port = "80"
    protocol = "HTTP"
    default_action {
        type = "forward"
        target_group_arn = aws_lb_target_group.two-tier-lb-tg.arn
    }
    }

    # create target group
    resource "aws_lb_target_group" "two-tier-web-loadb-tg" {
    name     = "target"
    depends_on = [ aws_vpc.two-tier-vpc ]
    port     = "80"
    protocol = "HTTP"
    vpc_id   = aws_vpc.two-tier-vpc.id
    }


    # attachment
    resource "aws_lb_target_group_attachment" "two-tier-web-loadb-tg-attch-1" {
    depends_on = [aws_security_group.two-tier-ec2-sg]
    target_group_arn = aws_lb_target_group.two-tier-web-loadb-tg.arn
    target_id        = aws_instance.two-tier-web-server-1.id
    port             = 80
    }

    resource "aws_lb_target_group_attachment" "two-tier-web-loadb-tg-attch-2" {
    depends_on = [aws_security_group.two-tier-ec2-sg]
    target_group_arn = aws_lb_target_group.two-tier-web-loadb-tg.arn
    target_id        = aws_instance.two-tier-web-server-2.id
    port             = 80
    }

    # Subnet group database
    resource "aws_db_subnet_group" "two-tier-db-sub" {
    name = "two-tier-db-sub"
    subnet_ids = [aws_subnet.two-tier-prv-sub-1.id, aws_subnet.two-tier-prv-sub-2.id]
    }
