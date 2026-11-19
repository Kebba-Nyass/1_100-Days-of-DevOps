# Day 8: Install Ansible

## Task
Install `ansible` version `4.7.0` on `Jump host` using `pip3` only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

## Why this matters
During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with `Ansible` for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use `jump host` as an Ansible controller to test different kind of tasks on rest of the servers.

## Steps

### Step 1

Confirm you're on the right server and user:

```bash
thor@jump-host ~$  whoami
thor
thor@jump-host ~$  hostname
jump-host
```
### Step 2 Switch to Root User

Gain root privileges to install Ansible globally: 
```bash
thor@jump-host ~$ sudo -i
```
### Step 3 Verify pip3 Installation

Check if pip3 is installed: 
```bash
[root@jump-host ~]# pip3 --version
pip 21.3.1 from /usr/lib/python3.9/site-packages/pip (python 3.9)
```
### Step 4 Install Ansible 4.7.0

Run the following command to install Ansible: 
```bash
[root@jump-host ~]# sudo pip3 install ansible==4.7.0
```
### Step 5 Create a Symlink for Global Access

Ensure that the Ansible binary is available globally by creating a symlink: 
```bash
[root@jump-host ~]# sudo ln -s /usr/local/bin/ansible /usr/bin/ansible
```
### Step 6 Verify Installation

Check if Ansible is installed correctly and accessible: 
```bash
[root@jump-host ~]#  ansible --version
ansible [core 2.11.12] 
  config file = None
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.9/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /bin/ansible
  python version = 3.9.19 (main, Jun 11 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)]
  jinja version = 3.1.6
  libyaml = True
```
Exit from the root user: 
```bash
[root@jump-host ~]# exit
```

Check Ansible version again:
```bash
thor@jump-host ~$ ansible --version
ansible [core 2.11.12] 
  config file = None
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.9/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /bin/ansible
  python version = 3.9.19 (main, Jun 11 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)]
  jinja version = 3.1.6
  libyaml = True
```