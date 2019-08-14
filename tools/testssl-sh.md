## testssh.sh

``` bash
site='https://www.google.com/'

filename="testssl_$(echo "$site" | sed 's_https*://__;s_/.*__')"
~/tools/testssl.sh/testssl.sh -oA "$filename" "$site"
```
