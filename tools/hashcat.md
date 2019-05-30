# hashcat

* [formats](https://hashcat.net/wiki/doku.php?id=example_hashes)

## Quick RTFM

Example with a wordlist:

``` bash
hashcat -w 4 -O -a 1000 --username -a 0 -r rules/best64.rule ntds/users_ntlm_1000.hash wordlists/rockyou.txt
```

Example with a mask (aka "smart brute force"):

``` bash
hashcat -w 4 -O -a 1000 --username -a 3 --increment ntds/users_ntlm_1000.hash -1 cC -2 oO0 -3 ?d?s ?1?2nt?2s?2?d?d?3?3?3?3
```

Example to show the result:

``` bash
hashcat -m 1000 --show ntds/users_ntlm_1000.hash
```

Options breakdown:

* `-w 4`: enable the max speed. Strongly recommended on your crack rig but do not use on your laptop unless you want your screen to lag.
* `-O`: Enable optimized kernels (limits password length). Mostly recommended unless you know what you are doing.
* `-m 1000`: mandatory, specify the hash type. See [the wiki](https://hashcat.net/wiki/doku.php?id=example_hashes) for a complete list.
* `--username`: use it if your hashlist is `jdoe_adm:51831bda51454aecb5d924d0dd12df8f` instead of `51831bda51454aecb5d924d0dd12df8f` to keep track of the username using this password.
* `-a X`: specify the attack type. See [the wiki](https://hashcat.net/wiki/doku.php?id=hashcat#supported_attack_modes) for all attack modes.
* `-r best64.rule`: in dictionary attack, specify the rule set to use.
* `--increment`: in brute force mode, tell hashcat to start with one character, then two, then three, etc.
* `-1 cC`: Create a "variable" which could take `c` and `C`, so the candidate will be `contoso` and `Contoso`.
* `?1?2nt?2s?2?d?d?3?3?3?3`: the mask to use. Replace any `?1`, `?2`, `?3` and `?4` by their values (see above), and `?d` by all digits, `?l` by lowercase letters, `?u` by uppercase, and `?s` by special characters. See [the wiki](https://hashcat.net/wiki/doku.php?id=mask_attack) for more.

## external functions

Require a hash file named like: `user_ntlm_1000.hash`

* starts with `user` if it contains usernames;
* ends with `<num>` which is the hash format (see above);
* `*.hash` files contain the hashes, whereas `*.show` files contain the `--show` counterpart.

``` bash
function print-stats-crack() {
    find . -name '*.hash' | while read fil ; do
        fil_show="${fil%.*}.show"
        if [[ -f "$fil_show" ]] ; then
            echo "($(($(wc -l "$fil_show"| grep -o "^[0-9]*")*100/$(wc -l "$fil" | grep -o "^[0-9]*"))) %) $fil"
        else
            echo "( 0 %) $fil"
        fi
    done
}

function hashcat-show-all() {
    find . -name '*.hash' | while read i ; do
        num="$(echo "$i" | grep -o '_[0-9]*.hash' | grep -o '[0-9]*')"
        ishow="${i%.*}.show"
        if basename "$i" | grep -q "^user" ; then
            user='--username'
        else
            user=''
        fi
        $(echo "hashcat -m $num $user --show $i") > "$ishow"
    done
}
```

## old work with SAP (almost deprecated)

//Toute ressemblance avec des missions ou des comptes admin existants ou ayant existés serait purement fortuite.//

``` bash
$ cat sapEtoile_bcode
SAP*:SAP*$9D7FAA188FE794B6

$ cat sapEtoile_passcode
SAP*:SAP*$AA89EF4EEFD6967F5257A45443F9E8FAC90D4D73

$ cat dico
06071992
PASSWORD
admin
```

Le BCODE (7700 - SAP CODVN B - genre de MD5 salé avec le username) n'est pas sensible à la casse et ne prend que les 8 premiers caractères. Il est donc rapide de casser d'abord ce hash afin d'avoir des infos sur le password réel. Ensuite, on peut lancer un crack sur le PASSCODE (7800 - SAP CODVN F/G - genre de SHA-1 salé avec le username) qui ira beaucoup plus vite car le workload sera réduit.

Dans un premier temps, on lance une attaque par dico avec des règles sur le BCODE :

``` bash
/root/oclhashcat/oclHashcat-1.31/oclHashcat64.bin --gpu-temp-retain=89 --gpu-temp-abort=95 --username -m 7700 -r rules/d3ad0ne.rule sapEtoile_bcode dico
```

S'il ne tombe pas, on fait un brute-force simple sur toutes les combinaisons. Ce n'est pas très long car il sera forcément ≥ 8 char.

``` bash
/root/oclhashcat/oclHashcat-1.31/oclHashcat64.bin --gpu-temp-retain=89 --gpu-temp-abort=95 --username -m 7700 --increment -a 3 sapEtoile_bcode -1 '?u?d?s' '?1?1?1?1?1?1?1?1'
```

Une fois que le BCODE est tombé, on se base sur ces infos pour cracker le PASSCODE. On affiche le BCODE trouvé puis on attaque d'abord par dico puis par force brute sur 14 caractères s'il n'est pas trouvé. Cette dernière attaque est réalisable car il n'y a que 4 caractères variable (majuscule/minuscule). En effet, oclHashcat ne permet que de configurer 4 masques maximum.

``` bash
$ /root/oclhashcat/oclHashcat-1.31/oclHashcat64.bin --gpu-temp-retain=89 --gpu-temp-abort=95 --username -m 7700 --show sapEtoile_bcode | tail -n +3
SAP*:SAP*$9D7FAA188FE794B6:BN42LS83

$ echo 'BN42LS83' > sapEtoile_dico ; /root/oclhashcat/oclHashcat-1.31/oclHashcat64.bin --gpu-temp-retain=89 --gpu-temp-abort=95 --username -m 7800 -r rules/d3ad0ne.rule sapEtoile_passcode sapEtoile_dico

$ /root/oclhashcat/oclHashcat-1.31/oclHashcat64.bin --gpu-temp-retain=89 --gpu-temp-abort=95 --username -m 7800 --increment -a 3 sapEtoile_passcode -1 bB -2 nN -3 lL -4 sS '?1?242?3?483?a?a?a?a?a?a'
```

C'est un peu long mais il devrait finir par tomber.

Conclusion : Une grosse liste de hash sera longue à casser car les hash sont salés à partir du username. Cependant, si le BCODE est activé, il permet d'avoir une bonne idée du password pour accélerer la phase de crack sur le PASSCODE. C'est donc assez rapide d'obtenir le mot de passe de n'importe quel compte avec cette méthode. Il est donc recommandé de désactiver le BCODE si possible (n'est présent que pour des questions de rétro-compatibilité).
