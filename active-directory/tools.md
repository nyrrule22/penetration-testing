# Tools

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
