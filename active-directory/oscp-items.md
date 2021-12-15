# OSCP Items

### 3rd Party Practice

* [x] THM - Active Directory Basics&#x20;
* [x] THM - Attacktive Directory&#x20;
* [x] THM - Attacking Kerberos&#x20;
* [x] THM - Post Exploitation Basics&#x20;
* [ ] TCM - PEH&#x20;
* [ ] THM - Holo, Wreath, Throwback (paid)&#x20;
* [ ] PG Practice - Heist, Hutch, Vault&#x20;
* [ ] HTB - Paths: Intro to Dante; Active Directory 101;
* [ ] HTB - ippsec.rocks walkthroughs

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
