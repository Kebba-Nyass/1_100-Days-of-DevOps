# Day 1
## Create a user named 'john' with a non-interactive shell on 'App Server 3'.

> ssh into the App server 3 using banner's login details.
```bash
    thor@jump-host ~$ ssh banner@stapp03
```
```bash
    [banner@stapp03 ~]$ whoami
    banner
    [banner@stapp03 ~]$ hostname
    stapp03
```
> It prompt for password, which I entered and finally login to the App Server 3.

```bash
    banner@stapp03 ~]$ sudo useradd -s /sbin/nologin john
```
> It prompts for steve's password which I entered and the account was created. Late>
```bash
    grep john /etc/passwd
    output:
    john:x:1001:1001::/home/john:/sbin/nologin
```





