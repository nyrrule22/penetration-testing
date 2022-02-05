# OSCP Items

### 3rd Party Practice

* [x] THM - Active Directory Basics&#x20;
* [x] THM - Attacktive Directory&#x20;
* [x] THM - Attacking Kerberos&#x20;
* [x] THM - Post Exploitation Basics&#x20;
* [x] THM - VulnNet: Roasted
* [x] THM - VulnNet: Active
* [x] THM - RazorBlack
* [x] THM - Enterprise
* [x] THM - Zero Logon
* [x] TCM - PEH
* [x] PWK - Active Directory Module
* [ ] THM - Holo
* [ ] THM - Wreath
* [ ] ~~THM - Throwback (paid)~~
* [x] PWK - Lateral Movement
* [ ] PWK - Assembling the Pieces
* [ ] ~~PG Practice - Heist, Hutch, Vault~~&#x20;
* [x] HTB - AD 101: Forest, Sauna, Active with IppSec walkthroughs
* [x] HTB - ippsec.rocks: Fuse
* [x] HTB - AD 101: Resolute, Cascade
* [ ] HTB - ippsec.rocks: Monteverde, Intelligence, APT
* [ ] HTB - AD 101: Blackfield, Reel, Sizzle, Mantis, Multimaster with IppSec walkthroughs
* [ ] HTB - Tracks: Intro to Dante

### Cheat Sheets

#### Mind Maps

AD - [https://www.xmind.net/m/5dypm8/](https://www.xmind.net/m/5dypm8/)

General Enumeration - [https://www.xmind.net/m/QsNUEz/](https://www.xmind.net/m/QsNUEz/)

[https://www.thehacker.recipes/](https://www.thehacker.recipes)

[https://www.ired.team/](https://www.ired.team)

#### WADComs

[https://wadcoms.github.io/](https://wadcoms.github.io)

#### Methodology

[https://book.hacktricks.xyz/windows/active-directory-methodology](https://book.hacktricks.xyz/windows/active-directory-methodology)

#### Resources

[https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)

[https://zer1t0.gitlab.io/posts/attacking\_ad/](https://zer1t0.gitlab.io/posts/attacking\_ad/)

[https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a)

#### Writeups

[https://infosecwriteups.com/active-directory-penetration-testing-cheatsheet-5f45aa5b44ff](https://infosecwriteups.com/active-directory-penetration-testing-cheatsheet-5f45aa5b44ff)

[https://n0h4ts.medium.com/passing-the-new-oscp-b9fcd434ade7](https://n0h4ts.medium.com/passing-the-new-oscp-b9fcd434ade7)

### FAQ

#### Will Offensive Security release an exam and lab report template for the new exam?

* This is a work in progress and we will share a new report template within a week or two.

#### How can I practice Active Directory?

* Read the corresponding Module on Active Directory&#x20;
* Read the final Module of the PEN-200 Course Material (Assembling the Pieces: Penetration Test Breakdown)
* Follow along and perform all the steps against the Sandbox.local Active Directory environment
* Begin enumerating the PWK labs. Locate and attack all Active Directory sets within the labs.

#### Is there any pivoting required for the Active Directory machines on the exam?

* There may be pivoting required. Anything in the course material is subject to being on the exam.

#### The exam in the past has required that we read the proof from the desktop location, not somewhere else. What does this mean for PowerShell Remoting? Is PSSession going to count as a shell?

* Yes, PowerShell Core counts as an interactive shell and is allowed on the exam.

#### Which tools are allowed for the new exam?

* All tools that do not perform any restricted actions are allowed on the exam.
  * BloodHound
  * SharpHound
  * PowerShell Empire
  * Covenant&#x20;
  * Powerview
  * Rubeus
  * evil-winrm
  * Responder (Spoofing is not allowed in the labs or on the exam)
  * Crackmapexec
  * Mimikatz
