## BloodHound

I'm used to extract the information using SharpHound.

### Extraction with SharpHound.exe

Updated version: <https://github.com/BloodHoundAD/BloodHound/tree/master/Ingestors>

From a Windows powershell prompt on a Windows host using the .NET 3.5 framework:
```
runas /netonly /user:<domain>\<user> 'powershell.exe'
```

In the new term just launched:


```
.\SharpHound.exe -d contoso.local -c All -v --statusinterval 10000
```

or

```
.\SharpHound.exe --DomainController 192.168.1.1 -C All -d contoso.local --statusinterval 15000 -v
```


### Import with BloodHound

#### Neo4J Database

Pick the Community Server for your platform: <https://neo4j.com/download-thanks/?edition=community&release=3.5.0&flavour=unix>

Change default password for user `neo4j`: `./bin/neo4j-admin set-initial-password P@$$w0rd`

Run it: `./bin/neo4j console`

#### BloodHound

* Launch
* Connect with previously configured password
* Import data (the zip of json files)
* Profit!


