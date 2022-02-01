# Windows

## Manual Enumeration

### System

```powershell
systeminfo  # host name, OS Name, OS version, etc…
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
wmic [command]
```

### User

```powershell
whoami
whoami /priv
whoami /groups
net user  # show users that are on the machine
net user [user]  # get information about a specific user
net localgroup administrator  # get users that belong to the administrator group
```

### Network

```powershell
ipconfig
ipconfig /all
arp -a
route print
netstat -ano
```

### Password Hunting

```powershell
findstr /si password *.txt *.ini *.config *xml  # searches from the directory for the word password in all the specified files
dir secret.doc /s /p  # searches recursively for a file called secret.doc
```

### A/V and Firewall Enumeration

```powershell
sc query windefend
sc queryex type= service
netsh advfirewall firewall dump
netsh firewall show state
netsh firewall show config
```

## Automated Enumeration

### Executables

* winPEAS
* seatbelt.exe
* Watson.exe
* SharpUp.exe

### PowerShell

* Sherlock.ps1
* PowerUp.ps1
* jaws-enum.ps1

### Other

* windows-exploit-suggester.py
* Exploit Suggester (Metasploit)

## Escalation Path Techniques

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)

### Kernel Exploits

### Service Exploits

### Registry Exploits

### Passwords

### Scheduled Tasks

### Insecure GUI Apps

### Startup Apps

### Installed Apps

### Hot Potato

### Token Impersonation

### Port Forwarding

### WSL

### getsystem

### RunAs

### DLL Hijacking
