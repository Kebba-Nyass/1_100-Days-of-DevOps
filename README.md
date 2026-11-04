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

