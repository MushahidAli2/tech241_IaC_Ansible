# Nginx YAML playbook
```yaml

# YAML file start with --- thre dashes
# Why playbooks
# Create a playbook to install nginx in web node
---

# Which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes
# admin access (sudo)
  become: true
# add the instructions  -  install nginx on web agent
  tasks:
  - name: Installing nginx
    apt: pkg=nginx state=present
# check the status of nginx - ensure is actively running

# adhoc command to check the status

```
# Move app from controller to WEB node
do `git clone` first, and than run this playbook
```yaml 
---
- name: Install npm
  hosts: web
  become: true

  tasks:
    - name: Update apt package cache
      apt:
        update_cache: yes

    - name: Install npm
      apt:
        name: npm
        state: present

    - name: Copy Application Data
      copy:
        src: /sparta-app
        dest: /home/ubuntu

```
# Automating launch of app

```yaml
---

# Which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes
# admin access (sudo)
  become: true
# add the instructions  -  install node 12 with pm2 and run the app
  tasks:
  - name: Installing node v 12the gog key for nodejs
    apt_key:
      url: "https://deb.nodesource.com/gpgkey/nodesource.gpg.key"
      state: present

  - name: Add NodeSource repository
    apt_repository:
      repo: "deb https://deb.nodesource.com/node_12.x {{ ansible_distribution_release }} main"
      state: present

  - name: Install Node.js
    apt:
      name: nodejs
      state: present
      update_cache: yes

  - name: Install PM2
    npm:
      name: pm2
      global: yes
      state: present
      version: "4.5.6"

  - name: Install app dependencies
    command: npm install
    args:
      chdir: "/home/ubuntu/sparta-app/app"

  - name: Stop PM2 processes
    shell: pm2 kill

  - name: Start the Node.js app with force re-execution
    command: pm2 start app.js
    args:
      chdir: "/home/ubuntu/sparta-app/app"

```
# Playbook to install Mongodb
This will only work after doing update and upgrade on your DB node
```yaml
---

# create a playbook to install mongodb in db-instance



# host

- hosts: db



# get logs

  gather_facts: yes



# admin access

  become: true



# provide instructions - task: install mongodb

  tasks:



# install mongodb

  - name: Installing MongoDB

    apt: pkg=mongodb state=present



# ensure the db is running



# check the status if its running with adhoc commands

```
check status: ` sudo ansible db -a "systemctl status mongodb"`