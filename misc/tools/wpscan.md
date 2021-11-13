# wpscan

## Basics

```bash
# Basic Scan
wpscan --url http://10.10.248.119 -o wpscan.log
# Enumerate vulnerable plugins, themes, and users
wpscan --url http://10.10.248.119 -e vp,vt,u -o wpscan.log --api-token <>
# Brute Forcing Login
wpscan --url http://10.10.128.156/ -U users.txt -P /usr/share/wordlists/rockyou.txt -vv
```
