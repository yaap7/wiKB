# Python3

## Standard Types

### dict

```
if 'key' in tab:
    print(tab['key'])
```

### bytes

```python
a = b'abc'

a[0]
97

a[0:1]
b'a'

a.hex()
'616263'
```

## argparse

Simple example with optionnal argument and multiple value argument:

```
argParser = argparse.ArgumentParser(description='Port scan using censys database')
argParser.add_argument('filter', nargs='+', help='Censys filter to search for. Can be a list of IPs.')
argParser.add_argument('-c', '--csv', dest='csv', help='Turn on CSV output', action='store_true')
args = argParser.parse_args()
```



More complex example with mandatory arguments depending on the request type:

```
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


## logging

Example to log on stdout instead of the default stderr and to enable DEBUG messages if `args.debug` is enabled:

```
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

## colors

`termcolor`, `colorama` or without any dependance with: https://stackoverflow.com/questions/287871/print-in-terminal-with-colors/21786287#21786287


```
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

```
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