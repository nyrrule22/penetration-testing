# Post-Compromise Enumeration

## PowerView Overview

## Domain Enumeration with PowerView

#### Example

```powershell
C:\Users\fcastle\Desktop>powershell -ep bypass
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\fcastle\Desktop> . .\PowerView.ps1
```

#### Get-NetDomain

Get information about the Domain

```powershell
PS C:\Users\fcastle\Desktop> Get-NetDomain

Forest                  : MARVEL.local
DomainControllers       : {HYDRA-DC.MARVEL.local}
Children                : {}
DomainMode              : Unknown
DomainModeLevel         : 7
Parent                  :
PdcRoleOwner            : HYDRA-DC.MARVEL.local
RidRoleOwner            : HYDRA-DC.MARVEL.local
InfrastructureRoleOwner : HYDRA-DC.MARVEL.local
Name                    : MARVEL.local
```

#### Get-NetDomainController

Get specific Domain Controllers

```powershell
PS C:\Users\fcastle\Desktop> Get-NetDomainController

Forest                     : MARVEL.local
CurrentTime                : 1/20/2022 1:57:31 AM
HighestCommittedUsn        : 53277
OSVersion                  : Windows Server 2019 Standard Evaluation
Roles                      : {SchemaRole, NamingRole, PdcRole, RidRole...}
Domain                     : MARVEL.local
IPAddress                  : 192.168.72.137
SiteName                   : Default-First-Site-Name
SyncFromAllServersCallback :
InboundConnections         : {}
OutboundConnections        : {}
Name                       : HYDRA-DC.MARVEL.local
Partitions                 : {DC=MARVEL,DC=local, CN=Configuration,DC=MARVEL,DC=local, CN=Schema,CN=Configuration,DC=MARVEL,DC=local, DC=DomainDnsZones,DC=MARVEL,DC=local...}
```

#### Get-DomainPolicy

Get all policies in the Domain

```powershell
PS C:\Users\fcastle\Desktop> Get-DomainPolicy

Name                           Value
----                           -----
Kerberos Policy                {MaxTicketAge, MaxServiceAge, MaxClockSkew, MaxRenewAge...}
System Access                  {MinimumPasswordAge, MaximumPasswordAge, LockoutBadCount, PasswordComplexity...}
Version                        {Revision, signature}
Registry Values                {MACHINE\System\CurrentControlSet\Control\Lsa\NoLMHash}
Unicode                        {Unicode}
```

```powershell
PS C:\Users\fcastle\Desktop> (Get-DomainPolicy)."System Access"
Name Value
---- -----
MinimumPasswordAge {1} 
MaximumPasswordAge {42} 
LockoutBadCount {0} 
PasswordComplexity {1} 
RequireLogonToChangePassword {0} 
LSAAnonymousNameLookup {0} 
ForceLogoffWhenHourExpire {0} 
PasswordHistorySize {24} 
ClearTextPassword {0} 
MinimumPasswordLength {7}
```

#### Get-NetUser

Get a bunch of user details

```powershell
PS C:\Users\fcastle\Desktop> Get-NetUser
...
```

```powershell
PS C:\Users\fcastle\Desktop> Get-NetUser | select cn
cn
--
Administrator 
Guest 
krbtgt 
Frank 
Castle 
Tony Stark 
Wade Wilson 
SQL Service
```

[https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)

## Bloodhound Overview and Setup

## Grabbing Data with Invoke-Bloodhound

## Enumerating Domain Data with Bloodhound
