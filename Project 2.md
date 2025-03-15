## PROJECT 2

# AUTOMATION OF ELITE CORPORATION INFRASTRUCTURE USING TERRAFORM.

# What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool that allows you to define, provision, and manage cloud infrastructure automatically using configuration files. It is developed by HashiCorp and supports multiple cloud providers like AWS, Azure, and Google Cloud.

# Benefits of Automating EC2 Instances with Terraform
Efficiency: Automates provisioning, reducing manual work.
Scalability: Easily deploy multiple instances by modifying configurations.
Consistency: Infrastructure as Code (IaC) ensures identical setups across  environments.
Version Control: Terraform configurations can be stored in Git, allowing easy tracking  of changes.
Cost Optimization: Quickly destroy unused resources, reducing unnecessary cloud  expenses.


# Project Task Requirements
Use Terraform to provision an EC2 instance in AWS.
Deploy the instance in a specific AWS region (e.g., us-east-1).
Use variables instead of hardcoded values for (AWS region, Instance type {e.g., t2.micro},Key pair name)
Use Terraform State Management
After applying the Terraform configuration, verify the Terraform state to ensure the infrastructure is being tracked.
Modify and Destroy Infrastructure
Modify the instance by changing the instance type and re-applying the configuration.
Destroy the infrastructure once verification is complete.

# Objective
The goal of this task is to automate the provisioning and management of AWS infrastructure using Terraform, eliminating manual deployment efforts and ensuring consistency, scalability, and reliability.

# Installation of Tools required:
Before starting, ensure you have the following installed:
Visual Studio Code (VS Code) – The code editor.
Terraform – Infrastructure as Code tool (Download Terraform).
AWS CLI – To authenticate with AWS (Install AWS CLI).
IAM User & Access Keys and Secret key – Required to manage AWS resources using Terraform.

# Create a Project Folder
Open VS Code and create a new folder (e.g.EliteCorp-P2,).
Inside the folder, create a file named (main.tf, Variable.tf)

![pic](<img/Screenshot (915).png>)

# Configure AWS Credentials
Before launching an EC2 instance, configure your AWS credentials using this  command shown below: 

aws configure 

terraform -v (if already install)

You'll be prompted to enter:
AWS Access Key ID
AWS Secret Access Key
Region (e.g., us-east-1)
Output format (JSON )

![pic](<img/Screenshot (914).png>)
![pic](<img>)

# Write a Terraform configuration file (main.tf) to define an EC2 instance running Ubuntu

![pic](<img/Screenshot (899).png>)

Note: after editing the variable file comeback and reference the variables in main.tf instead of hardcoded values.

![pic](<img/Screenshot (909).png>)

# Instead of hardcoding values, use Terraform variables to make the configuration reusable and flexible.
Create a variables.tf file:

![pic](<img>)

![pic](<img>)

Now, reference these variables in main.tf instead of hardcoded values.




# Automate Deployment with Terraform
Initialize Terraform: terraform init
Plan the deployment: terraform plan (preview changes before applying).
Apply the configuration: terraform apply -auto-approve (creates the EC2 instance).


![pic](<img>)

![pic](<img>)

![pic](<img>)

![pic](<img>)


# Using Terraform State Management
Is for Terraform to maintains the state of the deployed infrastructure to track changes and ensure consistency.

Initialize Terraform:
terraform init

![pic](<img/Screenshot (901).png>)

Apply the configuration:
terraform apply

![pic](<img/Screenshot (916).png>)

Verify Terraform state:
terraform state list

![pic](<img/Screenshot (917).png>)

# Modify and Destroy Infrastructure
Once the EC2 instance is deployed, the next step is to modify it and later destroy it when no longer needed.

# Modify the Instance Type
Change the instance type in variables.tf:
default = "t2.medium"  # Updated from t2.micro

![pic](<img/Screenshot (926).png>)


Reapply Terraform to update the infrastructure:
terraform apply

![pic](<img/Screenshot (921).png>)

Elite corporation AWS EC2 instance type has changed from t2.micro to t2.medium

![pic](<img/Screenshot (923).png>)

Destroy the Infrastructure
When the testing is complete, clean up the environment by running:
terraform destroy 

![pic](<img/Screenshot (924).png>)

![pic](<img/Screenshot (925).png>)