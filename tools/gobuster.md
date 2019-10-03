## gobuster

https://github.com/OJ/gobuster

### Simple run

``` bash
site='http://www.example.com:8080/console/app/'

s="$(echo "$site" | sed 's_https*://__;s_[:/]_-_g;s_-*$__g')"

~/tools/gobuster/main -a 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3569.0 Safari/537.36' -e -f -k -l -m dir -o "$(date '+%Y-%m-%d_%H-%M-%S')_${s}_dir.log" -u "$site" -w ~/tools/fuzzdb/discovery/predictable-filepaths/filename-dirname-bruteforce/raft-small-directories-lowercase.txt

~/tools/gobuster/main -a 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3569.0 Safari/537.36' -e -k -l -m dir -o "$(date '+%Y-%m-%d_%H-%M-%S')_${s}_files.log" -u "$site" -w /home/gg/tools/fuzzdb/discovery/predictable-filepaths/filename-dirname-bruteforce/raft-small-files-lowercase.txt
```

### Special Configuration

In case of "Wildcard response", it is possible to continue using `-fw` switch and adding `-s` for special codes.

For example:

``` txt
[-] Wildcard response found: https://www.example.com/65a4270-281a-47b2-b00e-8bd3bc0fd3f => 302
[!] To force processing of Wildcard responses, specify the '-fw' switch.
```

``` bash
~/tools/gobuster/main -a 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3569.0 Safari/537.36' -e -f -k -l -m dir -o "$(date '+%Y-%m-%d_%H-%M-%S')_${s}_dir.log" -u "$site" -w /home/gg/tools/fuzzdb/discovery/predictable-filepaths/filename-dirname-bruteforce/raft-small-directories-lowercase.txt -fw -s '200,204,301,307,403'
```
