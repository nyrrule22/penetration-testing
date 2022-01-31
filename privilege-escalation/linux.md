# Linux

## Manual Enumeration

### System

```bash
hostname
uname -a
cat  /proc/version
cat /etc/issue
lscpu
ps aux
cat /etc/crontab
crontab -e  # check for a specific user
cat /var/log/syslog | grep root
find / -type f -perm -4000 2>/dev/null, find / -perm -u=s -type f 2>/dev/null  # SUID
find / -type f -perm -2000 2>/dev/null  # GUID
getcap -r / 2>/dev/null
cat /etc/passwd
cat /etc/shadow
ls -l /etc/sudoers  # Check if we can write to the file
```

### User

```bash
whoami
id
sudo -l
cat /etc/passwd
cat /etc/shadow
cat /etc/group
history
```

### Network

```bash
ifconfig / ip a s
route
arp -a / ip neigh
netstat -ano / ss -an | grep LISTEN
```

### Password Hunting

```bash
grep --color=auto -rnw '/' -ie "PASSW" --color=always 2> /dev/null
locate password | more
find / -name id_rsa 2>/dev/null
find / -name authorized_keys 2>/dev/null
```

## Automated Enumeration

### Scripts

* LinPEAS
* LinEnum
* linux-exploit-suggester
* Linuxprivchecker.py
* linux-smare-enumeration (lse)
* pspy

## Escalation Path Techniques

### Sudo

### SUID

### Capabilities

### Cron Jobs

### Weak File Permissions

### Passswords & Keys

### NFS

### Service Exploits

### Docker

### Kernel Exploits
