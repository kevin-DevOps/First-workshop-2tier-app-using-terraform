+++
title = "Let's deploy!"
date = 2024-09-01T10:12:06+07:00
weight = 6
chapter = true
pre = "<b>2.6 </b>"
+++
## Let's deploy!
Make sure you have already inserted your AWS credentials and are operating from the root directory before starting these Terraform commands.

## 1. terraform init
The terraform init the command is used to initialize a new or existing Terraform configuration. This command downloads the required provider plugins and sets up the backend for storing state.
![init](/images/0001-tf.png)
terraform init

## 2. terraform plan

The terraform plan the command is used to create an execution plan for the Terraform configuration. This command shows what resources Terraform will create, modify, or delete when applied.
![plan](/images/0003-tf.png)
terraform plan

## 3. terraform apply

The terraform apply the command is used to apply the Terraform configuration and create or modify resources in the target environment.
![apply](/images/0004-tf.png)
terraform apply
