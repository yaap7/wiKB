# Network Management on Linux

## Basic Configuration

Set an IP address with a default gateway.

``` bash
sudo ip a add dev eth0 10.0.0.5/24
sudo ip route add default via 10.0.0.1
```

## Route Management

Show route table.

``` bash
ip route
```

Add/delete a default gateway.

``` bash
sudo ip route add default via 10.0.0.1
sudo ip route del default
```

Add/delete a specific route.

``` bash
sudo ip route add 10.20.0.0/24 via 10.0.0.1
sudo ip route del 10.20.0.0/24 via 10.0.0.1
```

## VLAN Management

Install the required kernel module.

``` bash
sudo aptitude install vlan
```

Load the kernel module.

``` bash
sudo modprobe 8021q
```

Add a virtual interface using a VLAN tag.

``` bash
sudo vconfig add eth0 45
```

Request an IP address for this VLAN.

``` bash
sudo dhclient -v eth0.45
```

Get information about current VLAN configuration.

``` bash
sudo cat /proc/net/vlan/config
```

Release an IP address obtained via DHCP

``` bash
sudo dhclient -r eth0.45
```

Remove a VLAN from an interface.

``` bash
sudo vconfig rem eth0.45
```

Unload the module.

``` bash
sudo rmmod 8021q
```

## Wi-Fi Management

### Connect to a WPA Wi-Fi using wpa_supplicant

``` bash
wpa_passphrase 'ESSID' 'W1fi_P@$$w0rd' > essid.conf
sudo wpa_supplicant -c essid.conf -i wlan0
```

### Lower Level Access

Get hardware info.

``` bash
iw dev wlan0 info
```

Change the card mode (monitor/managed).

``` bash
iw dev wlan0 set type monitor
iw dev wlan0 set type managed
```

or

``` bash
iwconfig wlan0 mode monitor
iwconfig wlan0 mode managed
```

Wi-Fi scans.

``` bash
iwlist wlan0 scan
```

or

``` bash
nmcli device wifi list
```

or

``` bash
# optionnal: airmon-ng check wlan0 or airmon-ng check kill wlan0
airmon-ng start wlan0
airodump -i wlan0
```

#### NetworkManager

Tell NetworkManager to not manage an interface:

``` bash
nmcli device set wlan0 managed off
nmcli device set wlan0 managed on
```
