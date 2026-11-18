# Day 7: Linux SSH Authentication

## Task

Set up a password-less authentication from user `thor` on jump host to all app servers through their respective sudo users.

## Why this matters

The system admins team of `xFusionCorp Industries` has set up some scripts on `jump host` that run on regular intervals and perform operations on all app servers in `Stratos Datacenter`. To make these scripts work properly we need to make sure the `thor` user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e `tony` for app server 1).

## Steps

### Step 1

#### 1. Generate a key pair for thor (skip if ~/.ssh/id_rsa already exists)

```bash
thor@jump-host ~$ ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/id_rsa
```

### Step 2

#### Copy the public key to app server 1's sudo user

```bash
thor@jump-host ~$ ssh-copy-id tony@stapp01     # App Server 1
```

you'll be prompted for the password once.

Verify that the login works without a password prompt

```bash
thor@jump-host ~$ ssh tony@stapp01
```

Confirm you're on the right server and user:

```bash
[tony@stapp01 ~]$ whoami
tony
[tony@stapp01 ~]$ hostname
stappo1
```

### Step 3

#### Copy the public key to app server 2's sudo user

```bash
thor@jump-host ~$ ssh-copy-id steve@stapp02 # App Server 2
```

you'll be prompted for the password once.

Verify that the login works without a password prompt

```bash
thor@jump-host ~$ ssh steve@stapp02
```

Confirm you're on the right server and user:

```bash
[steve@stapp02 ~]$ whoami
steve
[steve@stapp02 ~]$ hostname
stappo2
```

### Step 4

#### Copy the public key to app server 3's sudo user

```bash
thor@jump-host ~$ ssh-copy-id banner@stapp03 # App Server 3
```

you'll be prompted for the password once.

Verify that the login works without a password prompt

```bash
thor@jump-host ~$ ssh banner@stapp03
```

Confirm you're on the right server and user:

```bash
[banner@stapp03 ~]$ whoami
banner
[banner@stapp03 ~]$ hostname
stappo3
```
