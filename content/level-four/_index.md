+++
title = "Clean up resource"
date = 2024-09-01T10:12:06+07:00
weight = 5
chapter = true
pre = "<b>4. </b>"
+++

# **Clean up resource**
When working with Terraform to manage your infrastructure, it’s important to properly clean up resources when they are no longer needed. This helps avoid unnecessary costs and ensures that your environment remains organized. Here's how to clean up resources effectively using Terraform:

1. Review the Infrastructure
Before you proceed with cleaning up, ensure that you review the resources currently deployed. This will help you avoid accidentally deleting important resources. You can list all resources in your Terraform state file with the following command:

        terraform state list

2. Run Terraform Plan
To see what changes will be made before actually applying them, use the terraform plan command. This will show you which resources will be destroyed:

        terraform plan -destroy

Review the output to ensure that only the desired resources will be deleted.

3. Destroy Resources
To remove all the resources defined in your Terraform configuration, use the terraform destroy command. This will prompt you to confirm the destruction of resources:
        terraform destroy
If you want to skip the confirmation prompt, you can use the -auto-approve flag:
        terraform destroy -auto-approve

4. Clean Up Terraform State
After destroying the resources, you may want to clean up your local Terraform state files. Ensure that you have backed up any necessary state files before doing so. You can remove the state files manually or use the following command to remove the local .terraform directory:
        rm -rf .terraform

5. Remove Configuration Files
If you are finished with the project, you might also want to remove the Terraform configuration files (*.tf). Simply delete these files from your project directory:
        rm *.tf

6. Check for Orphaned Resources
Occasionally, some resources might not be properly removed due to dependencies or other issues. Verify manually in your cloud provider’s management console to ensure that no orphaned resources are left behind.

Summary
Cleaning up resources is an essential part of managing infrastructure with Terraform. By following these steps, you can ensure that your resources are properly removed, avoiding unnecessary costs and maintaining a tidy environment.

