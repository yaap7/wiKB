# How To Find a Wi-Fi Password in Cleartext

## Linux

``` bash
cat /etc/NetworkManager/system-connections/"<YourWPA2Network>"
```

## Windows

``` cmd
C:\ netsh WLAN show profile name="<YourWPA2Network>" key=clear
```

## MAC OS

``` bash
security find-generic-password -ga "<YourWPA2Network>" | grep "password:"
```


