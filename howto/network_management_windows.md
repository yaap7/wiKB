# Network Management on Windows

## Basic Configuration

Set an IP address with a default gateway and two DNS servers.

```
netsh interface ip set address "connection name" static 192.168.0.101 255.255.255.0 192.168.0.1
netsh interface ip add dns "connection name" 208.67.222.222
netsh interface ip add dns "connection name" 208.67.220.220 index=2
```

## Route Management

Show route table.

```
route print
```

Add/delete a default gateway.

```
route add 0.0.0.0 mask 0.0.0.0 10.0.0.1
route delete 0.0.0.0 mask 0.0.0.0 10.0.0.1
```

Add/delete a specific route.

```
route add 10.20.0.0 mask 255.255.255.0 10.0.0.1
route delete 10.20.0.0 mask 255.255.255.0 10.0.0.1
```
