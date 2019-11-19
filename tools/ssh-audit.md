## ssh-audit

[https://github.com/arthepsy/ssh-audit](https://github.com/arthepsy/ssh-audit)

```
mkdir ssh-audit
cat cibles-ssh.txt | while read host ; do
    ~/tools/ssh-audit/ssh-audit.py -n -v "$host" | tee "ssh-audit/$host.log"
done
```

TODO : faire un script qui lit les logs de cette commande et qui sort une feuille Excel.



