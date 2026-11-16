# Day 5: SELinux Installation and Configuration

## Task
1. Install the required `SELinux` packages.

1. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

1. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

1. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.

## Why this matters
Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for `App server 1` in the `Stratos Datacenter:`
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
### 2. Install the SELinux packages
```bash
[tony@stapp01 ~]$ sudo dnf install -y selinux-policy selinux-policy-targeted libselinux-utils
```
### 3. Set SELinux to disabled in the config (no reboot)
```bash
[tony@stapp01 ~]$ sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
[tony@stapp01 ~]$ grep -E '^SELINUX=' /etc/selinux/config    # should print SELINUX=disabled
```
### Verify
```bash
[tony@stapp01 ~]$ getenforce
Disabled
```