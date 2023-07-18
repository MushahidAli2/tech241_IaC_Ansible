
# Infrastructure as Code (IaC) Documentation

## Table of Contents
- [Infrastructure as Code (IaC) Documentation](#infrastructure-as-code-iac-documentation)
  - [Table of Contents](#table-of-contents)
  - [1. Introduction to Infrastructure as Code (IaC)](#1-introduction-to-infrastructure-as-code-iac)
  - [2. Why Use IaC?](#2-why-use-iac)
  - [3. Tools for IaC](#3-tools-for-iac)
  - [4. Configuration Management with Ansible](#4-configuration-management-with-ansible)
  - [5. Why Use Ansible?](#5-why-use-ansible)
  - [6. Creating an Ansible Control Machine in AWS](#6-creating-an-ansible-control-machine-in-aws)
    - [6.1. Launching an EC2 Instance for Ansible Control Machine](#61-launching-an-ec2-instance-for-ansible-control-machine)
    - [6.2. Adding SSH Key Pair to the Control Machine](#62-adding-ssh-key-pair-to-the-control-machine)
    - [6.3. Preparing the Ansible Control Machine](#63-preparing-the-ansible-control-machine)
    - [6.4. Installing Ansible](#64-installing-ansible)
  - [7. Ping Ansible Agents](#7-ping-ansible-agents)
    - [7.1. Preparing the Ansible Agents](#71-preparing-the-ansible-agents)
  - [7.2. Pinging Ansible Agents](#72-pinging-ansible-agents)

## 1. Introduction to Infrastructure as Code (IaC)

Infrastructure as Code (IaC) is a software engineering approach that involves managing and provisioning infrastructure through machine-readable definition files rather than manual processes. It allows developers and system administrators to manage infrastructure using version-controlled, repeatable, and consistent code. IaC treats infrastructure as software, enabling teams to manage, configure, and deploy resources more efficiently. test again

## 2. Why Use IaC?

Using IaC provides several benefits, including:
- **Consistency and Reproducibility**: IaC allows teams to create consistent and reproducible infrastructure environments across different stages of development and production.
- **Scalability and Flexibility**: Infrastructure can be easily scaled up or down, providing flexibility to meet changing demands.
- **Version Control**: Infrastructure configurations are version-controlled, enabling easy rollbacks and collaboration among team members.
- **Documentation**: Infrastructure code acts as documentation, making it easier to understand and maintain the system.
- **Automated Deployment**: IaC facilitates automated deployment, reducing human errors and speeding up the development process.
change
## 3. Tools for IaC

Several tools are available for implementing IaC, including:
- **Terraform**: A widely-used tool for provisioning and managing infrastructure across various cloud providers.
- **AWS CloudFormation**: Specific to Amazon Web Services (AWS), this tool allows users to define infrastructure as JSON or YAML templates.
- **Google Cloud Deployment Manager**: Similar to CloudFormation, but for Google Cloud Platform (GCP).
- **Azure Resource Manager**: Microsoft's IaC tool for managing resources in Azure.

## 4. Configuration Management with Ansible

Configuration management is the process of managing and maintaining the desired state of an infrastructure's configuration. Ansible is a powerful open-source configuration management tool that automates the provisioning, configuration, and management of infrastructure resources.

## 5. Why Use Ansible?

Some advantages of using Ansible include:
- **Agentless Architecture**: Ansible uses SSH to connect to remote hosts, eliminating the need for agents on managed systems.
- **Simple and Human-readable Syntax**: Ansible playbooks are written in YAML, making them easy to understand and maintain.
- **Idempotent Execution**: Ansible ensures that applying configurations multiple times produces the same result, avoiding unexpected changes.
- **Large Community and Extensibility**: Ansible has a vast user community and supports a wide range of modules and plugins.

## 6. Creating an Ansible Control Machine in AWS

![Ansible diagram.jpg](images%2FAnsible%20diagram.jpg)

### 6.1. Launching an EC2 Instance for Ansible Control Machine

1. Log in to the AWS Management Console.
2. Navigate to the EC2 Dashboard.
3. Click "Launch Instance" and select an appropriate Amazon Machine Image (AMI) for the control machine (e.g., Amazon Linux, Ubuntu, CentOS).
4. Choose an instance type based on your requirements.
5. Configure the instance details (e.g., VPC, subnet, security groups).
6. Add storage as per your needs.
7. Configure any additional details, such as tags.
8. Review and launch the instance.
9. Select an existing key pair or create a new one to access the instance via SSH.

### 6.2. Adding SSH Key Pair to the Control Machine

1. If you are creating a new key pair, AWS will provide a .pem private key file for download.
2. Securely store the .pem file on your local machine.

### 6.3. Preparing the Ansible Control Machine

1. Open a terminal or command prompt on your local machine.
2. Change the permissions of the .pem file to allow read-only access: `chmod 400 /path/to/your-key.pem`.

### 6.4. Installing Ansible
1. install common software

`sudo apt-get install software-properties-common`

2. Add a repo for ansible

`sudo apt-add-repository ppa:ansible/ansible`

1. Connect to the Ansible node machine using SSH, to make sure it is running and has access to internet: 
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your-ansible-control-machine-ip
   ```

2. Update the package manager:
- For Ubuntu: `sudo apt update`
3. Install Ansible:
- For Ubuntu: `sudo apt install ansible -y`

## 7. Ping Ansible Agents
### 7.1. Preparing the Ansible Agents

1. Launch EC2 instances that you want to manage as Ansible agents.
2. Ensure that the security groups of these instances allow inbound SSH (port 22) traffic from the Ansible control machine's IP.

## 7.2. Pinging Ansible Agents

1. Open a terminal on the Ansible control machine.
2. Navigate to the directory where your Ansible playbooks will reside (e.g.,` /etc/ansible`).
3. Open an inventory file (e.g., `hosts`) and add the IP addresses or DNS names of your Ansible agents:
![pingcommand.png](images%2Fpingcommand.png)

```yaml
[web]
  
ec2-instance ansible_host=52.16.232.170 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem
```
4. Test the connectivity to Ansible agents using the ping module:
```python
sudo ansible web -m ping
```
![pong response.png](images%2Fpong%20response.png)

That's it! You have successfully set up Ansible on your Ubuntu 18.04 EC2 instance and tested the connectivity to your Ansible agents. You can now proceed to create Ansible playbooks and manage your infrastructure as code.