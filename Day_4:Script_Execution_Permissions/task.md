# Day 4: Script Execution Permissions

## Task
Your task is to grant executable permissions to the `/tmp/xfusioncorp.sh` script on `App Server 1`. Additionally, ensure that all users have the capability to execute it (any user must be able to run it directly, for example `/tmp/xfusioncorp.sh`).

## Why this matters
In a bid to automate backup processes, the `xFusionCorp Industries` sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on `App Server 1` within the Stratos Datacenter.

## Steps

### 1. SSH into App Server 1 as `tony`
```bash
thor@jump-host ~$ ssh tony@stapp01
```

Enter the password when prompted.

Confirm you're on the right server and user:

```bash
[tony@stapp01 ~]$ whoami
tony
[tony@stapp01 ~]$ hostname
stappo1
```
### 2. Making the Script Executable by all users
Checking the file's permission
```bash
[tony@stapp01 ~]$ ls -l /tmp/xfusioncorp.sh 
---------- 1 root root 40 Nov  8 20:38 /tmp/xfusioncorp.sh
```
Changing the file permission to make it executable to all users.
```bash
[tony@stapp01 ~]$ sudo chmod a+rx /tmp/xfusioncorp.sh
```
Rechecking the file permissions
```bash
[tony@stapp01 ~]$ ls -l /tmp/xfusioncorp.sh
-r-xr-xr-x 1 root root 40 Nov  8 20:38 /tmp/xfusioncorp.sh
```
### 3 Verify
```bash
[tony@stapp01 ~]$ /tmp/xfusioncorp.sh
Welcome To KodeKloud
```