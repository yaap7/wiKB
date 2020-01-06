# Liens par thème

Une liste de liens / tools mis de côté.

## Pentest interne

### tutos / méthodes

#### dump creds

* [2013 - Dumping Windows credentials](https://www.securusglobal.com/community/2013/12/20/dumping-windows-credentials/) : Méthodes et outils pour récupérer des hashs/passwords en environnement Windows.
* [2017 - NTLM relaying guide](https://byt3bl33d3r.github.io/practical-guide-to-ntlm-relaying-in-2017-aka-getting-a-foothold-in-under-5-minutes.html): Précisions sur les relais NTLM
* [univershell_2017_dpapi.pdf](http://www.synacktiv.ninja/ressources/univershell_2017_dpapi.pdf) : Présentation d'outils pour récupérer des secrets via DPAPI

#### bypass AV/EDR

* [2019 - Combining Direct System Calls and sRDI](https://outflank.nl/blog/2019/06/19/red-team-tactics-combining-direct-system-calls-and-srdi-to-bypass-av-edr/): Red Team Tactics: Combining Direct System Calls and sRDI to bypass AV/EDR

### tools

* [DeathStar with Empire](https://byt3bl33d3r.github.io/automating-the-empire-with-the-death-star-getting-domain-admin-with-a-push-of-a-button.html) : Présentation de DeathStar (byt3bl33d3r)
* [RedSnarf](https://github.com/nccgroup/redsnarf): Tool for retrieving hashes and credentials from Windows using OpSec Safe Techniques
* [applocker-bypass-installutil](https://pentestlab.blog/2017/05/08/applocker-bypass-installutil/)
* [DockerRootPlease](https://github.com/chrisfosterelli/dockerrootplease): Gain root shell on the hostOS, if you're a member of docker group
* [mimipenguin](https://github.com/huntergregal/mimipenguin): mimikatz for Linux

### upgrade your shells

### payloads

* [Undetectable Windows Payload Generation](https://github.com/nccgroup/Winpayloads): Undetectable Windows Payload Generation with extras Running on Python2.7
* [CactusTorch](https://github.com/mdsecactivebreach/CACTUSTORCH): A JavaScript and VBScript shellcode launcher. ([doc](https://www.mdsec.co.uk/2017/07/payload-generation-with-cactustorch/))

## OSINT

* [Sublist3r](https://github.com/aboul3la/Sublist3r) : énumérer les sous-domaines
* [DNStrails](https://dnstrails.com/) : Historique des entrées DNS, whois, de n'importe quel domaine.
* [Domain Names enumeration](https://blog.appsecco.com/a-penetration-testers-guide-to-sub-domain-enumeration-7d842d5570f6) : Techniques pour énumérer les sous domaines
* [DNSgrep](https://blog.erbbysam.com/index.php/2019/02/09/dnsgrep/) : outil pour pouvoir chercher efficacement dans la collection de [DNS passif de Rapid7](https://opendata.rapid7.com/sonar.fdns_v2/) (voir un serveur standalone [ici](https://dns.bufferover.run/dns?q=artichaut.com))
* [CTFR](https://github.com/UnaPibaGeek/ctfr) : Outil pour récupérer des sous domaines depuis les Certificate Transparency logs

## Red Team

### Set up the infrastructure

* [Red-Team-Infrastructure-Wiki](https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki) : Infrastructure pour le red team

### Badge cloning

* [RFID Hacking with The Proxmark 3](https://blog.kchung.co/rfid-hacking-with-the-proxmark-3/)

## Blue Team

* [HELK](https://github.com/Cyb3rWard0g/HELK): A Hunting ELK (Elasticsearch, Logstash, Kibana) with advanced analytic capabilities.

## Password Cracking

* [Real-Passwords](https://github.com/berzerk0/Probable-Wordlists/tree/master/Real-Passwords): Une belle liste de dico
* [pemcracker](https://github.com/bwall/pemcracker): Cracking PEM files

## Wi-Fi audits

* [eaphammer](https://github.com/s0lst1c3/eaphammer) = fake AP + responder
* [fluxion](https://github.com/FluxionNetwork/fluxion) : MitM WPA
* [Wifite](https://github.com/derv82/wifite2): Rewrite of the popular wireless network auditor, "wifite"
* [radius-audit](https://github.com/plorinquer/radius-audit): A RADIUS authentication server audit tool

* **Modern Wireless Tradecraft**: series of posts about Wi-Fi hacking technics by the creator of `eaphammer`:
  * [Part I - Basic Rogue AP Theory — Evil Twin and Karma Attacks](https://posts.specterops.io/modern-wireless-attacks-pt-i-basic-rogue-ap-theory-evil-twin-and-karma-attacks-35a8571550ee)
  * [Part II - MANA and Known Beacon Attacks](https://posts.specterops.io/modern-wireless-attacks-pt-ii-mana-and-known-beacon-attacks-97a359d385f9)
  * [Part III - Management Frame Access Control Lists (MFACLs)](https://posts.specterops.io/modern-wireless-tradecraft-pt-iii-management-frame-access-control-lists-mfacls-22ca7f314a38)
  * [Part IV - Tradecraft and Defensive Strategy](https://posts.specterops.io/modern-wireless-tradecraft-pt-iv-tradecraft-and-detection-d1a95da4bb4d)

## Web

### Tools

* [Autochrome](https://www.nccgroup.trust/us/about-us/newsroom-and-events/blog/2017/march/autochrome/): Chromium that does not pollute your Burp's logs
* [Quieter Firefox](https://www.blackhillsinfosec.com/towards-quieter-firefox/): Firefox that does not pollute your Burp's logs

### Cheat Sheets

* [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings): A list of useful payloads and bypass for Web Application Security and Pentest/CTF
* [HTTP Security Headers](https://blog.appcanary.com/2017/http-security-headers.html): Everything you need to know about HTTP security headers
* [CSRF cheat sheet](https://trustfoundry.net/cross-site-request-forgery-cheat-sheet/): a good CSRF cheat sheet
* [XSS Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet): a nice XSS cheat sheet by PortSwigger
* [JWT Attack Playbook](https://github.com/ticarpi/jwt_tool/wiki#menu): Tools and attacks on JWT (JSON Web Token)

## applications mobiles

### android

* [Anbox](http://www.omgubuntu.co.uk/2017/04/android-apps-linux-desktop-anbox) : faire tourner les applis android sous linux sans émulateur
* [Drozer](https://github.com/mwrlabs/drozer) : Un framework de tests de sécurité

### iOS

* [Needle](https://github.com/mwrlabs/needle) : Un framework de tests de sécurité

## Cryptographie

* [cryptol.net](http://cryptol.net/) : pour auditer les implémentations crypto
* [tamarin-prover](https://tamarin-prover.github.io/) : pour auditer les designs crypto
* [shattered](http://shattered.io/) : détails des collisions sur SHA-1

## DFIR

Digital Forensics and Incident Response

* [dfirtriage](https://github.com/travisfoley/dfirtriage): Digital forensic acquisition tool for Windows-based incident response
* [Kansa](https://github.com/davehull/Kansa): A modular incident response framework in Powershell

### Malware Sandboxes

* [https://any.run/](https://any.run/): Interactive Online Malware Sandbox

## Citrix, Remote Desktop Web Access, etc

* [Citrix escape](https://www.pentestpartners.com/security-blog/breaking-out-of-citrix-and-other-restricted-desktop-environments/): Many technics used to evade Citrix-like environments

## Audit de configuration

### Windows

* [Windows-Secure-Host-Baseline](https://github.com/nsacyber/Windows-Secure-Host-Baseline): Configuration guidance for implementing the Windows 10 and Windows Server 2016 DoD Secure Host Baseline settings. #nsacyber

### SSH

* [SSH Pentesting Guide](https://community.turgensec.com/ssh-hacking-guide/): Tools, technics, and vulnerabilities about SSH.

## Cloud

* [ScoutSuite](https://github.com/nccgroup/ScoutSuite): Multi-Cloud Security Auditing Tool

### AWS

TODO

### Azure/Office365

* [O365 Logs Scripts](https://github.com/renisac/O365-Management-and-Log-Scripts): Security-Focused O365 Management and Log Resources
* [O365-InvestigationTooling](https://github.com/OfficeDev/O365-InvestigationTooling): low-volume activity data acquisition from the Office 365 Management Activity API

## Architecture

### Kubernetes

* [Kubernetes Pentest Methodology - Part 1](https://www.cyberark.com/threat-research-blog/kubernetes-pentest-methodology-part-1/)
* [Kubernetes Pentest Methodology - Part 2](https://www.cyberark.com/threat-research-blog/kubernetes-pentest-methodology-part-2/)
* [Kubernetes Pentest Methodology - Part 3](https://www.cyberark.com/threat-research-blog/kubernetes-pentest-methodology-part-3/)

## Recommandations

* [Prevent Mimikatz attacks](https://medium.com/blue-team/preventing-mimikatz-attacks-ed283e7ebdd5)

### Microsoft recommendations

* [https://aka.ms/PasswordSprayBestPractices](https://aka.ms/PasswordSprayBestPractices)

## Dark Web

* [https://dark.fail/](https://dark.fail/): kind of dark web indexer
