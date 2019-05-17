## john


#### LM

```
$ john-1.7.9-jumbo-7/run/john --format=LM to_crack
Loaded 3 password hashes with no different salts (LM DES [128/128 BS AVX-16])
Remaining 2 password hashes with no different salts
PASSWOR          (Administrator:1)
D                (Administrator:2)
guesses: 2  time: 0:00:00:00 DONE (Thu Apr 23 15:24:10 2015)  c/s: 1792K  trying: POPING - A
Warning: passwords printed above might be partial
Use the "--show" option to display all of the cracked passwords reliably
```

#### NTLM

```
$ john-1.7.9-jumbo-7/run/john --format=NT to_crack
Loaded 2 password hashes with no different salts (NT MD4 [128/128 X2 SSE2-16])
`               (Guest)`
password         (Administrator)
guesses: 2  time: 0:00:00:00 DONE (Thu Apr 23 15:24:27 2015)  c/s: 90550  trying: 123456 - maggie
Use the "--show" option to display all of the cracked passwords reliably
```

#### netNTLMv2

```
$ head -1 hash.txt
username::DOMAIN:1122334455667788:4345143FC24C73B390CDDDF8A7FEC9A7:010100000<super long hash>000000
$ john --format=netntlmv2 --wordlist=/home/…/rockyou.txt --rules hash.txt
```




