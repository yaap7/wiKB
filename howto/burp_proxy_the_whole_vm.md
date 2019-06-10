# Proxyfier toutes les communications d'une VM avec Burp

La victime est une VM en mode "host only" mais ça serait pareil avec une machine sur un wifi ou un réseau cablé.

L'idée est de rajouter des règles de NAT pour rediriger uniquement le trafic voulu vers notre burp.

* Configurer Burp pour qu'il écoute en mode transparent (voir [ici](http://portswigger.net/burp/help/proxy_options_invisible.html)) pour comprendre pourquoi), pour cela il suffit de cocher la case "invisible" dans l'option du listener.
* autoriser l'IP forward :

``` bash
sudo bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
```

* rajouter des règles de firewall :

``` bash
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

* dans la VM il faut rajouter la route par défaut et peut-être le serveur DNS. Ouvrir un terminal administrateur puis :

`route add 0.0.0.0 mask 0.0.0.0 192.168.56.1`
`netsh interface ipv4 add dnsserver "Connexion au réseau local" address=10.35.12.2 index=1`

* profit!
