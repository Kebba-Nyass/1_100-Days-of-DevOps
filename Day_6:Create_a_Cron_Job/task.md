# Day 6: Create a Cron Job

## Task

- Install cronie package on all Nautilus app servers and start crond service.

- Add a cron _/5 _ \* \* \* echo hello > /tmp/cron_text for root user.

## Why this matters

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job.

## Steps

### Step 1

#### 1. SSH into App Server 1 as `tony`

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

#### 2. Install 'cronie' on app server 1 and start crond service

```bash
[tony@stapp01 ~]$ sudo dnf install -y cronie
[tony@stapp01 ~]$ sudo systemctl status crond
○ crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.se>
     Active: inactive (dead)
[tony@stapp01 ~]$ sudo systemctl enable --now crond
[tony@stapp01 ~]$ sudo systemctl status crond
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.se>
     Active: active (running) since Tue 2026-11-17 10>
   Main PID: 5849 (crond)
      Tasks: 1 (limit: 404712)
     Memory: 948.0K
        CPU: 2ms
     CGroup: /system.slice/crond.service
             └─5849 /usr/sbin/crond -n
```

#### 3. Add the cron job for root user

```bash
[tony@stapp01 ~]$ sudo crontab -u root -l
no crontab for root
[tony@stapp01 ~]$ echo '*/5 * * * * echo hello > /tmp/cron_text' | sudo crontab -u root -
[tony@stapp01 ~]$ sudo crontab -u root -l
*/5 * * * * echo hello > /tmp/cron_text
```

#### 4. To Verify, wait up to 5 minutes, then run:

```bash
[tony@stapp01 ~]$ cat /tmp/cron_text    # should print: hello
```

### Step 2

#### 1. SSH into App Server 2 as `steve`

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

#### 2. Install 'cronie' on app server 2 and start crond service

```bash
[steve@stapp02 ~]$ sudo dnf install -y cronie
[steve@stapp02 ~]$ sudo systemctl status crond
○ crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.se>
     Active: inactive (dead)
[steve@stapp02 ~]$ sudo systemctl enable --now crond
[steve@stapp02 ~]$ sudo systemctl status crond
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.se>
     Active: active (running) since Tue 2026-11-17 10>
   Main PID: 6982 (crond)
      Tasks: 1 (limit: 404712)
     Memory: 964.0K
        CPU: 2ms
     CGroup: /system.slice/crond.service
             └─6982 /usr/sbin/crond -n
```

#### 3. Add the cron job for root user

```bash
[tony@stapp01 ~]$ sudo crontab -u root -l
no crontab for root
[steve@stapp02 ~]$ echo '*/5 * * * * echo hello > /tmp/cron_text' | sudo crontab -u root -
[steve@stapp02 ~]$ sudo crontab -u root -l
*/5 * * * * echo hello > /tmp/cron_text
```

#### 4. To Verify, wait up to 5 minutes, then run:

```bash
[steve@stapp02 ~]$ cat /tmp/cron_text    # should print: hello
```

### Step 1

#### 1. SSH into App Server 3 as `banner`

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

#### 2. Install 'cronie' on app server 3 and start crond service

```bash
[banner@stapp03 ~]$ sudo dnf install -y cronie
[banner@stapp03 ~]$ sudo systemctl status crond
○ crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.se>
     Active: inactive (dead)
[banner@stapp03 ~]$ sudo systemctl enable --now crond
[banner@stapp03 ~]$ sudo systemctl status crond
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.se>
     Active: active (running) since Tue 2026-11-17 10>
   Main PID: 29208 (crond)
      Tasks: 1 (limit: 404712)
     Memory: 952.0K
        CPU: 2ms
     CGroup: /system.slice/crond.service
             └─29208 /usr/sbin/crond -n
```

#### 3. Add the cron job for root user

```bash
[tony@stapp01 ~]$ sudo crontab -u root -l
no crontab for root
[banner@stapp03 ~]$ echo '*/5 * * * * echo hello > /tmp/cron_text' | sudo crontab -u root -
[banner@stapp03 ~]$ sudo crontab -u root -l
*/5 * * * * echo hello > /tmp/cron_text
```

#### 4. To Verify, wait up to 5 minutes, then run:

```bash
[banner@stapp03 ~]$ cat /tmp/cron_text    # should print: hello
```
