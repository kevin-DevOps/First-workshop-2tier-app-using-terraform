+++
title = "Overview"
date = 2024-09-01T10:12:06+07:00
weight = 1
chapter = true
pre = "<b>1. </b>"
+++

# **Deploying FCJ Management Application with Auto Scaling Group**


What is AWS Two-Tier Architecture?
AWS two-tier architecture is a common deployment pattern used for web applications in the cloud. It consists of two main components or tiers:

## 1.Web Server Tier: 
This tier is responsible for serving web pages and handling user requests. It typically includes load balancers, web servers, and application servers. Let’s take a closer look at each of these components:
Load Balancers: A load balancer distributes incoming traffic to multiple web servers, improving the performance and availability of the application. AWS provides two types of load balancers: Application Load Balancers and Network Load Balancers.
Web Servers: Web servers are responsible for serving web pages to the user’s browser. They can handle static content such as HTML, CSS, and JavaScript. Web servers can also execute dynamic content, such as PHP, Python, or Ruby on Rails.
## 2.Database Tier: 
This tier is responsible for storing and managing data used by the web application. It typically includes a database server and associated storage. Let’s take a closer look at each of these components:

## 3.Database Server: 
A database server stores and manages data used by the application. AWS provides a variety of database options, including Amazon Relational Database Service (RDS), Amazon DynamoDB, and Amazon Aurora.
Storage: Storage is used by the database server to store data. AWS provides various storage options, such as Amazon Elastic Block Store (EBS), Amazon Simple Storage Service (S3), and Amazon Elastic File System (EFS).
The two tiers are separated by a firewall or security group to restrict access to the database server from the internet. This architecture is highly scalable, and fault-tolerant, and provides a separation of concerns between the web and database tiers.

## Why do We use Two-Tier Architecture?
There are several reasons why AWS two-tier architecture is a popular choice for deploying web applications in the cloud:

## 1.Scalability: 
The two-tier architecture allows each tier to scale independently. For example, if the web server tier needs to handle more traffic, additional web server instances can be added, and the load balancer can distribute traffic among them. If the database tier needs to handle more data, additional storage or database instances can be added.
## 2.Fault-tolerance: 
The two-tier architecture is designed to be fault-tolerant, with multiple web server and database server instances providing redundancy. If one server or instance fails, the load balancer can route traffic to other instances, maintaining application availability.
## 3.Security: 
The separation of the web server and database tiers allows for better security by restricting access to the database from the internet. By using a firewall or security group to separate the tiers, access to the database can be restricted to only authorized sources.
## 4.Performance: 
he use of load balancers in the web server tier can improve performance by distributing traffic among multiple web server instances. This can improve the responsiveness of the application and help to reduce latency.
Overall, AWS two-tier architecture provides a flexible and scalable approach to deploying web applications in the cloud, while also providing improved performance, fault tolerance, and security.

What is the difference between AWS Two-Tier and Three-Tire architecture?
The terms “Two-Tier” and “Three-Tier” refer to the architecture of an application or system. A tier is a logical or functional layer that provides a specific set of services or functionality.

In a Two-tier architecture, there are two tiers or layers: the client layer and the server layer. The client layer is typically a web or mobile application that communicates directly with the server layer, which consists of a database and application server.

In a Three-Tier architecture, there are three tiers or layers: the client layer, the application layer, and the database layer. The client layer communicates with the application layer, which contains the application server and business logic, and the application layer communicates with the database layer, which contains the database.

The main difference between Thw-Tier and Three-Tier architecture is the addition of an application layer in the latter. The application layer provides an additional level of abstraction between the client and the database, which can improve the scalability, security, and maintainability of the system.

In a Three-Tier architecture, the application layer can also provide load balancing and fault tolerance capabilities, which can help ensure the high availability of the system. However, a Three-Tier architecture can be more complex and expensive to implement compared to a Two-Tier architecture.

## Creating a Two-Tier Architecture in AWS Using Terraform:
In this article, I will use Terraform to create a two-tier architecture using AWS services. A two-tier architecture consists of a client layer and a database layer. Terraform is an open-source infrastructure as a code tool. With Terraform we can deploy cloud services by declaring the resources in a file.

Two-Tier Architecture:
![Logo của Hugo](/images/architecture.png)

## This two-tier architecture will contain the following components:

Deploy a VPC with CIDR 10.0.0.0/16
Within the VPC we will have 2 public subnets with CIDR 10.0.0.0/18 and 10.0.64.0/18. Each public subnet will be in a different Availability Zone for high availability
Create 2 private subnets with CIDR ‘10.0.128.0/18’ and ‘10.0.192.0/18’ and each will be in a different Availability Zone
Deploy RDS MySQL instance.
An Application load balancer that will direct traffic to the public subnets.
Deploy one EC2 instance in each public subnet for high availability.
Internet Gateway and Elastic IPs for EC2 instance.
## Prerequisites:
Install Terraform CLI
Install AWS CLI
AWS Account
Code Editor (I used VS Code for this deployment)
Reference: https://registry.terraform.io/




