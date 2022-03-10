# Share a Wired Connection via Wi-Fi

**... and how to listen all network connections passing through it.**

**Main goal**: An easy way to get in *physical* man-in-the-middle to be sure to never miss a packet from a victim connected to our Wi-Fi access point.

Network map:

``` text
              ___________________
              |      LAPTOP     |
INTERNET <--> | eth0      wlan0 | <--> Wi-Fi "test" : 10.42.42.0/24
              ---^----------^----
                DHCP    10.42.42.1

```

## Network Basic Management

``` bash
sudo systemctl stop NetworkManager.service
sudo dhclient -v eth0
sudo ip link set dev wlp1s0 up
sudo ip a add dev wlan0 10.42.42.1/24

```

## Wi-Fi Access Point Creation

``` bash
sudo aptitude install hostapd
```

In file `hostapd.conf`:

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

## DHCP on the Wi-Fi Interface

``` bash
sudo aptitude install isc-dhcp-server
```

In file `dhcpd.conf`:

``` text
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

## DNS on the Wi-Fi Interface

In file `dnsmasq.conf`:

``` text
interface=wlan0
no-dhcp-interface=wlan0
```

``` bash
sudo dnsmasq -x ./dnsmasq.pid -C ./dnsmasq.conf
```

## Firewall Configuration

To forward and NAT packets from the Wi-Fi.

``` bash
sudo bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
sudo iptables -I FORWARD -s 10.42.42.0/24 -j ACCEPT
sudo iptables -I FORWARD -d 10.42.42.0/24 -j ACCEPT
sudo iptables -A POSTROUTING -t nat -o eth0 -j MASQUERADE
```

## More Information

La page `hostapd` de la doc ubuntu-fr est très complète : [https://doc.ubuntu-fr.org/hostapd](https://doc.ubuntu-fr.org/hostapd)

## Going Further

Maybe you want to redirect HTTP(S) traffic to a proxy (Burp Suite in "invisible" mode?):

``` bash
sudo iptables -t nat -I PREROUTING -s 10.42.42.0/24 -p tcp --dport 443 -j DNAT --to-destination 10.42.42.1:8080
sudo iptables -t nat -I PREROUTING -s 10.42.42.0/24 -p tcp --dport 80 -j DNAT --to-destination 10.42.42.1:8080
```

Then configure Burp to listen in "invisible" mode on the `wlan0` interface.
