# Recover passwords and secrets of local accounts on Windows

## new - method 1

Il faut récupérer les fichiers `SAM`, `SYSTEM`, et `SECURITY` dans `WINDOWS/system32/config/` et les extraire avec `creddump7` :

``` bash
~/git/creddump7/pwdump.py SYSTEM SAM > pwdump.txt
~/git/creddump7/cachedump.py SYSTEM SECURITY true > cachedump.txt
~/git/creddump7/lsadump.py SYSTEM SECURITY true > lsadump.txt
```

Voir aussi RedSnarf

## old - method 2

``` bash
bkhive WINDOWS/system32/config/system ~/winXPAD_syskey.txt
samdump2 WINDOWS/system32/config/SAM ~/winXPAD_syskey.txt > ~/winXPAD_hashes.txt
```

Y'a plus qu'à cracker les hashes du fichier `~/winXPAD_hashes.txt` !
