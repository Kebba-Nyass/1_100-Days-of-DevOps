# Day 2: Temporary User Setup with Expiry

## Task

Create a user named `ravi` on `App Server 1` in Stratos Datacenter. Set the expiry date to `2027-01-28`, ensuring the user is created in lowercase as per standard protocol.

## Why this matters
As part of the temporary assignment to the `Nautilus` project, a developer named `ravi` requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed.

## Steps

### 1. SSH into App Server 3 as `tony`
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
### 2. Create the temporal user with expiry date
```bash
[tony@stapp01 ~]$ sudo useradd -e 2027-01-28 ravi
```
`sudo` prompts for tony's password. The `-e` sets the expiration date.

### 3. Verify
```bash
[tony@stapp01 ~]$ id ravi
uid=1001(ravi) gid=1001(ravi) groups=1001(ravi)
[tony@stapp01 ~]$ sudo chage -l ravi
Last password change                        : Nov 05, 2026
Password expires                            : never
Password inactive                           : never
Account expires                             : Jan 28, 2027
Minimum number of days between password change               : 0
Maximum number of days between password change               : 99999
Number of days of warning before password expires    : 7

```
The output of `chage -l` should show Account expires : `Jan 28, 2027`.
