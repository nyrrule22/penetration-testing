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

### Currently Logged in Users

### Enumeration through SPNs

## Authentication

## Lateral Movement Techniques
