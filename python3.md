# Python3

* [script stub](#script-stub)
* [Standard Types](#standard-types)
* [format](#format)
* [argparse](#argparse)
* [logging](#logging)
* [json](#json)
* [re](#re)
* [colors](#colors)
* [scapy](#scapy)

## script stub

``` python
#!/usr/bin/env python3

import argparse
import logging
import sys


def main(args):
    # start of program
    logging.info("hello world!")


if __name__ == "__main__":
    # arguments parsing
    argParser = argparse.ArgumentParser(description="Default description to replace.")
    argParser.add_argument("files", nargs="+", help="Input files to parse.")
    argParser.add_argument("-o", "--output", required=True, help="Output file")
    argParser.add_argument("-v", "--verbose", help="Turn on verbose messages", action="store_true")
    argParser.add_argument("-d", "--debug", help="Turn on debugging", action="store_true")
    args = argParser.parse_args()
    # logging configuration
    logger = logging.getLogger()
    handler = logging.StreamHandler(sys.stdout)
    if args.debug:
        logger.setLevel(logging.DEBUG)
        formatter = logging.Formatter(
            fmt="%(asctime)-19s %(levelname)-8s %(name)s %(message)s",
            datefmt="%Y-%m-%d_%H:%M:%S",
        )
    else:
        if args.verbose:
            logger.setLevel(logging.INFO)
        else:
            logger.setLevel(logging.WARNING)
        formatter = logging.Formatter(fmt="%(message)s", datefmt="%Y-%m-%d_%H:%M:%S")
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    main(args)
```

## Standard Types

### dict

``` python
tab = {'key' : 'value', 'name' : 'jack', 'city' : 'laval'}

if 'key' in tab:
  print(tab['key'])

for value in tab.values():
  print(value)

for key, value in tab.items():
  print('key = {}, value = {}'.format(key, value))
```

### bytes

``` python
>>> a = b'abc'
>>> a[0]
97
>>> a[0:1]
b'a'
>>> a.hex()
'616263'
>>> bytes(23) # probably not what you want
b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
>>> bytes([23]) # probably exactly what you want
b'\x17'
```

## debug_var

debug any (short) variable, human readable

``` python
def debug_var(var: Any, tabs: int = 0) -> str:
    retour = ""
    if var is None:
        retour = "<None>"
    elif isinstance(var, str) or isinstance(var, int) or isinstance(var, float):
        retour = str(var)
    elif isinstance(var, tuple):
        retour += "(\n"
        for i in var:
            retour += " " * tabs + f"  + {debug_var(i, tabs+2)}\n"
        retour += " " * tabs + ")"
    elif isinstance(var, list):
        retour += "[\n"
        for i in var:
            retour += " " * tabs + f"  - {debug_var(i, tabs+2)}\n"
        retour += " " * tabs + "]"
    elif isinstance(var, dict):
        retour += "{\n"
        for k, v in var.items():
            retour += (
                " " * tabs + f"  * {debug_var(k, tabs+2)}: {debug_var(v, tabs+2)}\n"
            )
        retour += " " * tabs + "}"
    else:
        retour += " " * tabs + f"### Type non supporté ({var.__class__})\n"
    return retour

# Usage:
print(debug_var(my_tab))
```

## format

See: [https://pyformat.info/](https://pyformat.info/)

## argparse

Simple example with optionnal argument and multiple value argument:

``` python
argParser = argparse.ArgumentParser(description='Port scan using censys database')
argParser.add_argument('filter', nargs='+', help='Censys filter to search for. Can be a list of IPs.')
argParser.add_argument('-c', '--csv', dest='csv', help='Turn on CSV output', action='store_true')
args = argParser.parse_args()
```

More complex example with mandatory arguments depending on the request type:

``` python
# Parse arguments
argParser = argparse.ArgumentParser(description="Active Directory LDAP Enumerator")
argParser.add_argument('-l', '--server', required=True, dest='ldap_server', help='IP address of the LDAP server.')
argParser.add_argument('-t', '--type', required=True, dest='request_type', help='Request type: info, whoami, search, trusts, TODO')
argParser.add_argument('-d', '--domain', dest='domain', help='Authentication account\'s FQDN. Example: "contoso.local".')
argParser.add_argument('-u', '--username', dest='username', help='Authentication account\'s username.')
argParser.add_argument('-p', '--password', dest='password', help='Authentication account\'s password.')
argParser.add_argument('-s', '--search-filter', dest='search_filter', help='Search filter (use LDAP format).')
argParser.add_argument('search_attributes', default='*', nargs='*', help='LDAP attributes to look for.')
argParser.add_argument('-z', '--size_limit', dest='size_limit', default=10, help='Size limit (default is server\'s limit).')
argParser.add_argument('-o', '--output', dest='output_file', help='Write results in specified file too.')
argParser.add_argument('-v', '--verbose', dest='verbosity', help='Turn on debug mode', action='store_true')
args = argParser.parse_args()

# Set mandatory arguments for each request_type
mandatory_arguments = {}
mandatory_arguments['info'] = []
mandatory_arguments['whoami'] = ['domain', 'username', 'password']
mandatory_arguments['search'] = ['domain', 'username', 'password', 'search_filter']
mandatory_arguments['trusts'] = ['domain', 'username', 'password']
mandatory_arguments['show'] = ['domain', 'username', 'password', 'search_filter']
if args.request_type not in mandatory_arguments.keys():
  argParser.error('request type must be one of: {}.'.format(', '.join(mandatory_arguments.keys())))
for mandatory_argument in mandatory_arguments[args.request_type]:
  if vars(args)[mandatory_argument] is None:
    argParser.error('{} argument is mandatory with request type = {}'.format(mandatory_argument, args.request_type))
```

## csv

Example that I use often to output result in a CSV file:

```python
fichier_resultat = args.output
logging.debug(f"Writing results in {fichier_resultat}")
with open(fichier_resultat, "w", newline="") as csvfile:
    writer = csv.DictWriter(csvfile, fieldnames=headers)
    writer.writeheader()
    writer.writerows(vulns)
logging.info(f"End of script. Results are in {fichier_resultat}")
```

## logging

Example to log on stdout instead of the default stderr and to enable DEBUG messages if `args.debug` is enabled:

``` python
logger = logging.getLogger()
handler = logging.StreamHandler(sys.stdout)
if args.debug:
  logger.setLevel(logging.DEBUG)
else:
  logger.setLevel(logging.INFO)
formatter = logging.Formatter(fmt='%(asctime)-19s %(levelname)-8s %(message)s', datefmt='%Y-%m-%d_%H:%M:%S')
handler.setFormatter(formatter)
logger.addHandler(handler)
```

## json

Pretty print

``` bash
echo '{"json":"obj"}' | python -m json.tool
```

``` python
import json

print(json.dumps(json.loads('["foo", {"bar":["baz", null, 1.0, 2]}]'), indent=2, sort_keys=True))
```

## re

How to use the Regular Expression module of python3.

``` python
import re

regex = re.compile(r'([A-Za-z]*):\s*(.*)')
data = 'Username:        \n        user3837'
matches = regex.findall(data)
print(matches)
```

``` text
[('Username', 'user3837')]
```

## colors

`termcolor`, `colorama` or without any dependance with: [https://stackoverflow.com/questions/287871/print-in-terminal-with-colors/21786287#21786287](https://stackoverflow.com/questions/287871/print-in-terminal-with-colors/21786287#21786287)

``` python
for style in range(8):
  for fg in range(30,38):
    s1 = ''
    for bg in range(40,48):
      format = ';'.join([str(style), str(fg), str(bg)])
      s1 += '\x1b[%sm %s \x1b[0m' % (format, format)
    print(s1)
  print('\n')
```

## scapy

Run on a pcap file to extract all TCP data in raw format.
It is then possible to run a `binwalk -e` to extract known file types.

``` python
#!/usr/bin/env python3

from scapy.all import *
import sys


def main():
  pkt_cap = rdpcap(sys.argv[1])
  for pkt in pkt_cap:
    sys.stdout.buffer.write(bytes(pkt[TCP]))
  sys.exit(0)


if __name__ == '__main__':
  main()
```

Run on a pcap file to extract all DNS queries and responses (might be enhanced)

``` python
#!/usr/bin/env python3

from scapy.all import *
import sys


def main():
  pkt_cap = rdpcap(sys.argv[1])
  for pkt in pkt_cap:
    if pkt.haslayer(DNSQR):
      query = pkt[DNSQR].qname.decode('utf-8')
      print('> {}'.format(query))
    if pkt.haslayer(DNSRR):
      response = pkt[DNSRR].rdata
      print('< {}'.format(response))
  sys.exit(0)

if __name__ == '__main__':
  main()
```
