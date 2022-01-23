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

## Lateral Movement Techniques
