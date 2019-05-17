## patator

https://github.com/lanjelot/patator

### SSH

```
patator.py ssh_login host=FILE0 0=targets_ips.txt user=root password='Passw0rd'
```

```
ip=192.168.0.1

~/tools/patator/patator.py ssh_login host=$ip user=FILE0 password=FILE1 0=/home/user/tools/fuzzdb/wordlists-user-passwd/unix-os/unix_users.txt 1=/home/user/tools/fuzzdb/wordlists-user-passwd/unix-os/unix_passwords.txt -x ignore:mesg='Authentication failed.' | tee patator_ssh_$ip_unix.log
```



