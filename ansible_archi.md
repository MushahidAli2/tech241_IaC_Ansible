# Ansible Architecture Setup

This repository provides instructions and scripts to set up an Ansible architecture on AWS. It includes steps for creating EC2 instances, configuring Ansible, and running playbooks and ad-hoc commands for automation.

## Step 1: Creating EC2 Instances on AWS

1. Create three EC2 instances on AWS:
   - One for the Ansible controller
   - Two for the application and database agent nodes
   - Ensure SSH security group rules are set
   - Use Ubuntu 18.04 as the operating system since it comes with preinstalled Python, which is the only dependency required for Ansible.

## Step 2: Installing Ansible on the Controller Node

1. SSH into the Ansible controller node.
2. Update and upgrade the system:

   ```bash
   sudo apt update -y
   sudo apt upgrade -y
   ```
3. Install Ansible using the PPA repository:
   ```bash
   sudo apt-add-repository ppa:ansible/ansible
   sudo apt install ansible -y
   sudo apt update -y
   ```
## Step 3: Configuring the Controller Node

   -  Create a .pem file named "tech241" in the ~/.ssh/ folder on the controller node.

## Step 4: Preparing the Agent Nodes

- SSH into the application and database agent nodes from the controller node.

- Update and upgrade the system to ensure they are available for connection:
  ```bash
  sudo apt update -y
  sudo apt upgrade -y
  ```
## Step 5: Configuring the Hosts File

   1.  Navigate to the default Ansible directory:
    ```bash
    cd /etc/ansible/
    ```
   2.  Edit the hosts file using the nano text editor:
   ```bash
   sudo nano hosts
   ```
   3. Add the IP addresses and SSH connection details for the app and db VMs:
   ```yaml
   [web]
   web-instance ansible_host=79.125.56.86 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem
   [db]
   db-instance ansible_host=54.229.227.188 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem
   ```
## Step 6: Testing the Connection

    Test the connection between the controller and agent nodes using the ping module:
    ```bash
     sudo ansible web -m ping
     sudo ansible db -m ping
    ```
## Ad-hoc Commands

Ad-hoc commands in Ansible are one-liners that can be executed directly from the command line without the need for separate playbooks or scripts. Here are some examples:

- Check the OS version on the agent node:
```bash
sudo ansible web -a "uname -a"
```

- Check the time zone on the agent node:
```bash
sudo ansible web -a "date"
```

- Check free space on the agent node:
```bash
sudo ansible web -a "free"
```

- List installed packages on the agent node:
```bash
sudo ansible web -a "ls -a"
```
## Playbooks - Automating Tasks

To automate tasks, we use playbooks. Here's an example playbook to install Nginx on the web node and configure its security group to allow port 80.

- Create a playbook named nginx.yml in the default location /etc/ansible:
```yaml
# why Yaml
# YAMl files start with --- tree dashes
# why playbooks
#create a playbook to install nginx in web node

---

# which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes

# admin access
  become: true
# add the instructions - install nginx - web
  tasks:
  - name: Installing Nginx
    apt: pkg=nginx state=present
# ensure the status of nginx is actively running

# adhoc command to check the status
```
- Execute the playbook using the following command:
```bahs
sudo ansible-playbook nginx.yml
```

The playbook will install Nginx on the web node, and the output will indicate success.