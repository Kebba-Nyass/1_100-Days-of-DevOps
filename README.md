# Day 1: Linux User Setup with Non-Interactive Shell

## Task

Create a user named `john` with a non-interactive shell on **App Server 3**.

## Why this matters

Service accounts and application users often need to exist on a system without anyone being able to log in as them. A non-interactive shell such as `/sbin/nologin` blocks interactive logins while still allowing the user to own files and run processes.

## Steps

### 1. SSH into App Server 3 as `banner`

```bash
thor@jump-host ~$ ssh banner@stapp03
```

Enter the password when prompted.

Confirm you're on the right server and user:

```bash
[banner@stapp03 ~]$ whoami
banner
[banner@stapp03 ~]$ hostname
stapp03
```

### 2. Create the user with a non-interactive shell

```bash
[banner@stapp03 ~]$ sudo useradd -s /sbin/nologin john
```

`sudo` prompts for banner's password. The `-s` flag sets the login shell.

### 3. Verify

```bash
grep john /etc/passwd
# john:x:1001:1001::/home/john:/sbin/nologin
```

The last field, `/sbin/nologin`, confirms the shell.

Optionally, confirm login is blocked:

```bash
sudo su - john
# This account is currently not available.
```

## Key takeaways

- `useradd -s <shell>` sets the user's login shell.
- `/sbin/nologin` denies interactive login.
- `/etc/passwd` is the quickest place to verify a user's shell.

---

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

# Day 3: Secure Root SSH Access

## Task

The task is to disable direct SSH root login on all app servers (`stapp01`, `stapp02` and `stapp03`) within the `Stratos Datacenter`. Make sure the change is applied to the running SSH service on each server.

## Why this matters

Following security audits, the `xFusionCorp Industries` security team has rolled out new protocols, including the restriction of direct root SSH login.

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

#### 1.1 Checking the current setting

```bash
[tony@stapp01 ~]$ sudo grep -rn "PermitRootLogin" /etc/ssh/sshd_config /etc/ssh/sshd_config.d/
    /etc/ssh/sshd_config:40:#PermitRootLogin prohibit-password
    /etc/ssh/sshd_config:90:# the setting of "PermitRootLogin without-password".
    /etc/ssh/sshd_config:131:PermitRootLogin yes
```

#### 1.2 If showed a PermitRootLogin `yes` line in a file under sshd_config.d/, it has to be change to `no` by editing the SSH daemon config either using nano or vim.

```bash
[tony@stapp01 ~]$ sudo nano /etc/ssh/sshd_config

# WARNING: 'UsePAM no' is not supported in RHEL and may cause several
# problems.
#UsePAM no

#AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
#X11Forwarding no
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
#PrintMotd yes
#PrintLastLog yes
#TCPKeepAlive yes
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#UseDNS no
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

# no default banner path
#Banner none

# override default of no subsystems
Subsystem       sftp    /usr/libexec/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#       X11Forwarding no
#       AllowTcpForwarding no
#       PermitTTY no
#       ForceCommand cvs server
PermitRootLogin no

```

#### 1.3 Validate the config and reload

```bash
# Syntax check (no output means OK)
[tony@stapp01 ~]$ sudo sshd -t

# Reload the service
[tony@stapp01 ~]$ sudo systemctl reload sshd
```

#### 1.4. Verify

Check the effective configuration (this reflects what the daemon will actually use, including drop-ins):

```bash
[tony@stapp01 ~]$ sudo sshd -T | grep -i permitrootlogin
Expected output: permitrootlogin no
```

---

### 2. SSH into App Server 2 as `steve`

```bash
thor@jump-host ~$ ssh steve@stapp02
```

Enter the password when prompted.

Confirm you're on the right server and user:

```bash
[steve@stapp02 ~]$ whoami
steve
[steve@stapp02 ~]$ hostname
stappo2
```

#### 2.1 Checking the current setting

```bash
[steve@stapp02 ~]$ sudo grep -rn "PermitRootLogin" /etc/ssh/sshd_config /etc/ssh/sshd_config.d/
    /etc/ssh/sshd_config:40:#PermitRootLogin prohibit-password
    /etc/ssh/sshd_config:90:# the setting of "PermitRootLogin without-password".
    /etc/ssh/sshd_config:131:PermitRootLogin yes
```

#### 2.2 If showed a PermitRootLogin `yes` line in a file under sshd_config.d/, it has to be change to `no` by editing the SSH daemon config either using nano or vim.

```bash
[steve@stapp02 ~]$ sudo vim /etc/ssh/sshd_config

# WARNING: 'UsePAM no' is not supported in RHEL and may cause several
# problems.
#UsePAM no

#AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
#X11Forwarding no
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
#PrintMotd yes
#PrintLastLog yes
#TCPKeepAlive yes
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#UseDNS no
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

# no default banner path
#Banner none

# override default of no subsystems
Subsystem       sftp    /usr/libexec/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#       X11Forwarding no
#       AllowTcpForwarding no
#       PermitTTY no
#       ForceCommand cvs server
PermitRootLogin no

```

#### 2.3 Validate the config and reload

```bash
# Syntax check (no output means OK)
[steve@stapp02 ~]$ sudo sshd -t

# Reload the service
[steve@stapp02 ~]$ sudo systemctl reload sshd
```

#### 2.4. Verify

Check the effective configuration (this reflects what the daemon will actually use, including drop-ins):

```bash
[steve@stapp02 ~]$ sudo sshd -T | grep -i permitrootlogin
Expected output: permitrootlogin no
```

---

### 3. SSH into App Server 3 as `banner`

```bash
thor@jump-host ~$ ssh banner@stapp03
```

Enter the password when prompted.

Confirm you're on the right server and user:

```bash
[banner@stapp03 ~]$ whoami
banner
[banner@stapp03 ~]$ hostname
stappo3
```

#### 3.1 Checking the current setting

```bash
[banner@stapp03 ~]$ sudo grep -rn "PermitRootLogin" /etc/ssh/sshd_config /etc/ssh/sshd_config.d/
    /etc/ssh/sshd_config:40:#PermitRootLogin prohibit-password
    /etc/ssh/sshd_config:90:# the setting of "PermitRootLogin without-password".
    /etc/ssh/sshd_config:131:PermitRootLogin yes
```

#### 3.2 If showed a PermitRootLogin `yes` line in a file under sshd_config.d/, it has to be change to `no` by editing the SSH daemon config either using nano or vim.

```bash
[banner@stapp03 ~]$ sudo vim /etc/ssh/sshd_config

# WARNING: 'UsePAM no' is not supported in RHEL and may cause several
# problems.
#UsePAM no

#AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
#X11Forwarding no
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
#PrintMotd yes
#PrintLastLog yes
#TCPKeepAlive yes
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#UseDNS no
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

# no default banner path
#Banner none

# override default of no subsystems
Subsystem       sftp    /usr/libexec/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#       X11Forwarding no
#       AllowTcpForwarding no
#       PermitTTY no
#       ForceCommand cvs server
PermitRootLogin no

```

#### 3.3 Validate the config and reload

```bash
# Syntax check (no output means OK)
[banner@stapp03 ~]$ sudo sshd -t

# Reload the service
[banner@stapp03 ~]$ sudo systemctl reload sshd
```

#### 3.4. Verify

Check the effective configuration (this reflects what the daemon will actually use, including drop-ins):

```bash
[banner@stapp03 ~]$ sudo sshd -T | grep -i permitrootlogin
Expected output: permitrootlogin no
```
