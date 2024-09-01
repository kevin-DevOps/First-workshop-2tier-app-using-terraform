+++
title = "Create db-resources.tf file"
date = 2024-09-01T10:12:06+07:00
weight = 5
chapter = true
pre = "<b>2.5 </b>"
+++

## 5. Create db-resources.tf file

The db-resources.tf file in Terraform is responsible for configuring the resources required to provision an RDS database in your AWS environment. This file typically includes the configuration of a MySQL RDS database, including the definition of the database instance class, allocated storage, and the desired backup window.

The RDS database defined in the db-resources.tf file may be configured with specific database instance classes and storage types, depending on the requirements of your application. Additionally, the file may specify the desired backup window for the RDS instance, which determines when automated backups are taken and how long they are retained.

Properly configuring the db-resources.tf file is crucial for ensuring that your RDS database is configured with the correct settings and backup policies. By defining the database instance class, allocated storage, and backup window, you can ensure that your RDS instance is optimized for performance, durability, and availability.

Overall, the db-resources.tf file is an essential part of any Terraform project that requires an RDS database. Properly configuring this file ensures that your RDS database is provisioned with the correct configurations, storage, backup settings, and any other desired policies. This helps to ensure the reliability and scalability of your application and minimize the risk of data loss.

    # RDS MYSQL database
    resource "aws_db_instance" "two-tier-db-1" {
    allocated_storage           = 5
    storage_type                = "gp2"
    engine                      = "mysql"
    engine_version              = "5.7.44"
    instance_class              = "db.t3.micro"
    db_subnet_group_name        = "two-tier-db-sub"
    vpc_security_group_ids      = [aws_security_group.two-tier-db-sg.id]
    parameter_group_name        = "default.mysql5.7"
    db_name                     = "two_tier_db1"
    username                    = "admin"
    password                    = "password"
    allow_major_version_upgrade = true
    auto_minor_version_upgrade  = true
    backup_retention_period     = 0
    multi_az                    = false
    skip_final_snapshot         = true
    }