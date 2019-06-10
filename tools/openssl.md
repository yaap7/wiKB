# openssl

## Create a simple PKI

How to create a CA certificate (self-signed) and a certificate for a web server signed by this CA.

### Create the CA

``` bash
openssl req -newkey rsa:8192 -nodes -x509 -days 3650 -extensions v3_ca -keyout cakey.pem -out cacert.pem
chmod 644 cacert.pem
chmod 400 cakey.pem
```

### Create the web server certificate

Write this configuration file in `req.conf`:

``` conf
[ req ]
default_bits       = 4096
distinguished_name = req_distinguished_name
req_extensions     = req_ext
[ req_distinguished_name ]
countryName                 = Country Name (2 letter code)
stateOrProvinceName         = State or Province Name (full name)
localityName               = Locality Name (eg, city)
organizationName           = Organization Name (eg, company)
commonName                 = Common Name (e.g. server FQDN or YOUR name)
[ req_ext ]
subjectAltName = @alt_names
[alt_names]
DNS.1   = bestflare.com
DNS.2   = usefulread.com
DNS.3   = chandank.com
```

Then create the certificate request:

``` bash
openssl req -new -out apache.csr -newkey rsa:4096 -days 365 -nodes -sha256 -keyout apache.key -config openssl.cnf
```

And sign it:

``` bash
openssl ca -config openssl.cnf -out apache.crt -extfile truc.cnf -in apache.csr
```
