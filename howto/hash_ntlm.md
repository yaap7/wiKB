# Create an NTLM hash from Linux

## one-liner

``` bash
echo -n secrEtPassw0rd | iconv -t utf16le | openssl md4
```

## script

``` bash
#!/bin/bash

if [[ "$#" -le 0 ]] ; then
    cat - | while read line ; do
        echo -n "$line" | iconv -t utf16le | openssl md4 | grep -o '[0-9a-f]\{32\}'
    done
fi

while [[ "$#" -gt 0 ]] ; do
    echo -n "$1" | iconv -t utf16le | openssl md4 | grep -o '[0-9a-f]\{32\}'
    shift
done
```

Show the NTLM hash of each line (from a file, or inline).

### Usages

``` bash
$ ./ntlmsum 'P@$$w0rd'
f56a8399599f1be040128b1dd9623c29
```

``` bash
$ cat pass.txt | ./ntlmsum 
f56a8399599f1be040128b1dd9623c29
```

``` bash
$ ./ntlmsum 'P@$$w0rd' 'Ub3r_$3cRe7'
f56a8399599f1be040128b1dd9623c29
733aac45c620a5c11c9e03a40262fc7c
```

``` bash
$ cat multipass.txt | ./ntlmsum
f56a8399599f1be040128b1dd9623c29
733aac45c620a5c11c9e03a40262fc7c
```
