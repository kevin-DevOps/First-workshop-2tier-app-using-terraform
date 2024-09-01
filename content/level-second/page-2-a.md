+++
title = "Create provider.tf file"
date = 2024-09-01T10:12:06+07:00
weight = 1
chapter = true
pre = "<b>2.1 </b>"
+++

## 1.Create provider.tf file
The provider.tf file in Terraform is a configuration file that specifies the cloud provider and its corresponding plugin that Terraform will use to manage resources in that provider.

In Terraform, a provider is a plugin that defines the set of resources and their properties that Terraform can manage in a specific cloud provider, such as AWS, Azure, Google Cloud, or others. The provider plugin is responsible for communicating with the API of the cloud provider, creating, updating, or deleting resources, and handling the authentication and access control.


    terraform {
    required_version = ">= 1.7.0, < 2.0.0"
    required_providers {
        aws = {
        source  = "hashicorp/aws"
        version = "~> 5.0"
        }
    }
    }

    # Configure the AWS Provider
    provider "aws" {
    region = "ap-southeast-1"
    }