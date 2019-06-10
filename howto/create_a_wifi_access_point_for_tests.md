# Créer une AP Wi-Fi pour les tests

Pour sniffer à coup sûr tout ce qui est envoyé/reçu par un smartphone ou n'importe quel IoT Wi-Fi.

On monte une AP avec service DHCP et on NAT les connexions au Wi-Fi pour leur donner accès à Internet via la connexion ethernet.

Plan réseau :

``` text
              ___________________
              |      LAPTOP     |
INTERNET <--> | eth0      wlan0 | <--> Wi-Fi "test" : 10.42.42.0/24
              ---^----------^----
                DHCP    10.42.42.1

```

## Gestion manuelle du réseau

``` bash
sudo systemctl stop NetworkManager.service
sudo dhclient -v eth0
sudo ip link set dev wlp1s0 up
sudo ip a add dev wlan0 10.42.42.1/24
```

## Création de l'AP

``` bash
sudo aptitude install hostapd
```

Fichier hostapd.conf:

``` text
interface=wlan0
ssid=test
hw_mode=g
channel=1
ignore_broadcast_ssid=0
wpa=1
wpa_passphrase=secretpassphrase
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP CCMP
```

``` bash
sudo hostapd ./hostapd.conf
```

## Activation du serveur DHCP

``` bash
sudo aptitude install isc-dhcp-server
```

Fichier dhcpd.conf:

``` bash
ddns-update-style none;
default-lease-time 600;
max-lease-time 7200;
log-facility local7;
subnet 10.42.42.0 netmask 255.255.255.0 {
  range 10.42.42.100 10.42.42.200;
  option routers 10.42.42.1;
  option subnet-mask 255.255.255.0;
  option broadcast-address 10.42.42.255;
  option domain-name-servers 10.42.42.1;
}
```

``` bash
sudo dhcpd -d -f -cf dhcpd.conf wlan0
```

## Activation du serveur DNS

Fichier dnsmasq.conf:

``` bash
interface=wlan0
no-dhcp-interface=wlan0
```

``` bash
sudo dnsmasq -x ./dnsmasq.pid -C ./dnsmasq.conf
```

## Ouverture des flux et NAT

``` bash
sudo bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
sudo iptables -I FORWARD -s 10.42.42.0/24 -j ACCEPT
sudo iptables -I FORWARD -d 10.42.42.0/24 -j ACCEPT
sudo iptables -A POSTROUTING -t nat -o eth0 -j MASQUERADE
```

## Pour plus d'infos

La page `hostapd` de la doc ubuntu-fr est très complète : [https://doc.ubuntu-fr.org/hostapd](https://doc.ubuntu-fr.org/hostapd)

## Pour aller plus loin

Il peut être utile de renvoyer le trafic à destination des ports 80 et 443 vers un burp afin de décoder le trafic HTTP[S].

``` bash
sudo iptables -t nat -I PREROUTING -s 10.42.42.0/24 -p tcp --dport 443 -j DNAT --to-destination 10.42.42.1:8080
sudo iptables -t nat -I PREROUTING -s 10.42.42.0/24 -p tcp --dport 80 -j DNAT --to-destination 10.42.42.1:8080
```

Puis faire écouter Burp sur l'interface wlan0 sur le port 8080.
