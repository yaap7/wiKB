## nmap

List scan (do resolve names) from a file containing IP addresses
`nmap -iL ips.txt -vv -sL`

SYN scan all port and output in the three major format
`nmap -iL ips.txt -vv -sS -p- -oA scan_syn_all_ports`

Lancer tous les scripts non DOS de récupération d'infos sur AD à l'aide d'un compte. (on peut aussi tester sans les args pour tester si c'est accessible en anonyme.)
`sudo nmap --script='smb-* and not smb-flood and not smb-brute' -p 445 192.168.0.2 -d --script-args smbuser=david,smbpass=password`

Create the HTML report from the XML output:
`xsltproc <nmap-output.xml> -o <nmap-output.html>`


### Convert to CSV

Convert the greppable output to CSV:
`cat nmap_output.gnmap | ~/tools/nmaptocsv/nmaptocsv.py -n | csvformat -q '"' -d ';' > nmap_output.csv`

of using `TAB` for easier Excel import:
`cat nmap_output.gnmap | ~/tools/nmaptocsv/nmaptocsv.py -n | csvformat -q '"' -d ';' -D '   ' > nmap_output.csv`

Requirements: [csvkit](https://csvkit.readthedocs.io/en/latest/) and [nmaptocsv](https://github.com/maaaaz/nmaptocsv).

### Convert to HTML

Easy to do with `xsltproc` (because the stylesheet is already included in the XML).

This version is using `parallel` to improve performance (and to learn `parallel` :p).

``` bash
find . -name '*.xml' | parallel xsltproc -o {.}.html {}
```
