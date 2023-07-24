# How to Add Environment Variables on Windows 11 for Terraform AWS Access

## Terraform Documentation
For further details and advanced usage of Terraform, you can refer to the official Terraform documentation at [Terraform Documentation](https://www.terraform.io/docs/index.html).

## Companies Using Terraform
Terraform is a widely adopted infrastructure as code tool used by various companies across different industries. Here are three big companies that currently use Terraform to manage their infrastructure:

1. **HashiCorp**: The company behind Terraform itself, HashiCorp, uses Terraform extensively to manage their infrastructure and cloud services.

2. **Adobe**: Adobe, a multinational software company known for its creative and digital experience products, relies on Terraform for their infrastructure automation.

3. **SAP**: SAP, a leading enterprise application software company, uses Terraform to automate their cloud infrastructure provisioning and management.

## Step 1: Open the Environment Variables Window
1. Press the `Windows Key + S` on your keyboard to open the search bar.
2. Type "Environment Variables" and click on "Edit the system environment variables."

## Step 2: Access System Properties
1. In the "System Properties" window that opens, click on the "Environment Variables" button at the bottom.

## Step 3: Add Environment Variables
1. In the "Environment Variables" window, under "User variables" section, click on "New..."
2. In the "New User Variable" dialog box, enter `AWS_ACCESS_KEY_ID` in the "Variable name" field.
3. In the "Variable value" field, paste your AWS Access Key ID.
4. Click "OK" to save the variable.

## Step 4: Add the Second Environment Variable
1. Click on "New..." again to add another variable.
2. In the "New User Variable" dialog box, enter `AWS_SECRET_ACCESS_KEY` in the "Variable name" field.
3. In the "Variable value" field, paste your AWS Secret Access Key.
4. Click "OK" to save the variable.

## Step 5: Verify Environment Variable Setup
1. To verify that the environment variables are set correctly, open a new Command Prompt or PowerShell window.
2. Type `echo %AWS_ACCESS_KEY_ID%` and press Enter. It should display your AWS Access Key ID.
3. Similarly, type `echo %AWS_SECRET_ACCESS_KEY%` and press Enter. It should display your AWS Secret Access Key.

## Running Terraform Commands on Git Bash
1. Open Git Bash.
2. Run `terraform --version` to check the installed Terraform version.

## Creating and Managing Terraform Files
1. Create a new directory named "terradorm-tech241" using the command: `mkdir terradorm-tech241`.
2. Navigate to the created directory: `cd terradorm-tech241`.
3. Create a new file named "main.tf" and edit it using the command: `nano main.tf`. Paste the following content into the file:

```hcl
# launch an ec2 on aws
# which cloud - aws
# terraform download required dependencies
# terraform init

# provider name
provider "aws" {
       # which part of aws
       region = "eu-west-1"
}

# Launch an ec2 in Ireland
resource "aws_instance" "app_instance" {
  # which machine/OS version etc. AMI-id ubuntu 18.04 LTS
  ami = "ami-id"

  # what type of instance - t2.micro
  instance_type = "t2.micro"

  # is the public IP required
  associate_public_ip_address = true

  # what would you like to name it tech241-mushahid-terraform-app
  tags = {
       Name = "tech241-mushahid-terraform-app"
  }
}
```
View the content of the "main.tf" file using the command: `cat main.tf`.

## Initializing Terraform and Running Plan

- Initialize Terraform to download the required dependencies with the command: `terraform init`.
- Edit the "main.tf" file using the command: `nano main.tf` (if needed).
- Plan the execution to see what changes will be made with the command: `terraform plan`.

## Destroying the Infrastructure

- Once you are done with your infrastructure, you can use the `terraform destroy` command to tear it down.
- Open the Git Bash or your terminal, navigate to the "terradorm-tech241" directory (if not already there).
- Run `terraform destroy`.
- Terraform will prompt you to confirm the destruction. Type `yes` and press Enter to proceed.
- Terraform will generate a destruction plan, showing which resources will be deleted and in what order.
- Once you confirm the plan, Terraform will start destroying the resources, and the cloud provider will delete them accordingly.
- After successful destruction, Terraform will update the state file to reflect the changes.

**Note:** Be cautious while using `terraform destroy`, as it will delete the resources you created. Ensure that you have the correct configuration and that you have confirmed the destruction plan before proceeding.


# Creating a Virtual Private Cloud (VPC) with Terraform on AWS

In this guide, we will walk you through the process of creating a Virtual Private Cloud (VPC) using Terraform on Amazon Web Services (AWS). We will explain the concept of CIDR blocks and provide a simple diagram to help you understand how VPCs work.

## Prerequisites

Before you begin, make sure you have the following:

- An AWS account with appropriate permissions to create VPC resources.
- Terraform installed on your local machine. You can download it from the official website: [Terraform Downloads](https://www.terraform.io/downloads.html).
- AWS CLI installed and configured with your AWS credentials.

## Understanding CIDR Blocks

CIDR (Classless Inter-Domain Routing) is a notation used to specify IP address ranges. CIDR blocks are used to define the IP address range for a VPC and its subnets. The CIDR block format is represented as `x.x.x.x/y`, where `x.x.x.x` is the IP address, and `y` is the prefix length. The prefix length indicates the number of bits in the IP address that are used for network identification.

For example, a CIDR block of `10.0.0.0/16` means that the VPC will have a range of IP addresses from `10.0.0.0` to `10.0.255.255`, and the first 16 bits (in this case, the first two octets) are used for network identification, leaving 16 bits for host addresses.

## Creating the VPC using Terraform

### Step 1: Initialize Terraform

Create a new directory for your Terraform configuration and create a file named `main.tf` inside it.

```hcl
# main.tf
provider "aws" {
  region = "us-east-1"  # Replace with your desired AWS region
}
```
Open your terminal, navigate to the directory containing the main.tf file, and run the following command to initialize Terraform:

```bash
terraform init
```