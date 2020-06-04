# Apache2

## Enable caching client-side

Enable the module:

``` bash
sudo a2enmod expires
sudo systemctl restart apache2
```

Then TODO

README: pourquoi j'avais du mettre mpm_event à la place de mpm_prefork déjà ?

## Enable HTTP/2

Example based on Ubuntu 18.04 and [this doc](https://gist.github.com/GAS85/8dadbcb3c9a7ecbcb6705530c1252831):

``` bash
sudo a2enmod http2
# but mpm_prefork is not compatible with HTTP/2
# so we need to use mpm_event instead of mpm_prefork
# but php-fpm is needed to use mpm_event
sudo apt install -y php-fpm
sudo a2dismod mpm_worker
sudo a2dismod php7.2
sudo a2dismod mpm_prefork
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php7.2-fpm
sudo a2enmod mpm_event
sudo systemctl restart apache2
```

Then add this line to your configuration or on a per-virtualhost basis:

``` conf
Protocols h2 http/1.1
```

## Enable Brotli Compression

Install prerequisites:

``` bash
sudo apt install -y brotli
sudo a2enmod brotli
sudo systemctl restart apache2
```

Then add brotli configuration inside virtualhost:

``` conf
<VirtualHost *:80>
[...]
    <IfModule mod_brotli.c>
        BrotliCompressionQuality 10
        #AddOutputFilterByType BROTLI_COMPRESS text/html text/plain text/xml text/css text/javascript application/javascript
        SetOutputFilter BROTLI_COMPRESS
        SetEnvIfNoCase Request_URI \.(?:gif|jpe?g|png)$ no-brotli
    </IfModule>
[...]
</VirtualHost>
```

And reload Apache2:

``` bash
sudo systemctl reload apache2
```

But beware of BREACH attacks when using compression and SSL/TLS.

## VirtualHost Example

Provides:

* Redirect HTTP to HTTPS
* Enable HSTS
* A+ on [SSLTest](https://www.ssllabs.com/ssltest/)
* HTTP/2 support
* Caching of static file on client side (1 month)
* Protect against directory listing
* Enable .htaccess
* no compression (beware of BREACH attacks in case of compression with TLS)

``` conf
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    # When redirecting everything you don't even need a DocumentRoot
    #DocumentRoot /var/www/example
    ServerName example.com
    ServerAlias www.example.com

    # Enable HTTP/2
    Protocols h2 http/1.1

    Redirect / https://example.com/
    # or
    # RewriteEngine on
    # RewriteCond %{SERVER_NAME} =example.com
    # RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]

    # logging
    ErrorLog ${APACHE_LOG_DIR}/error_example.log
    CustomLog ${APACHE_LOG_DIR}/access_example.log combined

    <Location "/">
        AllowMethods GET POST
    </Location>
</VirtualHost>

<IfModule mod_ssl.c>
    <VirtualHost *:443>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/example
        ServerName example.com
        ServerAlias www.example.com

        # Enable HTTP/2
        Protocols h2 http/1.1

        # logging
        ErrorLog ${APACHE_LOG_DIR}/error_example.log
        CustomLog ${APACHE_LOG_DIR}/access_example.log combined

        <Location "/">
            AllowMethods GET POST
        </Location>

        # disable directory listing but enable .htaccess
        <Directory /var/www/example>
            Options -Indexes
            AllowOverride all
        </Directory>

        # enable caching of static files client side
        <IfModule mod_expires.c>
            <FilesMatch "\.(jpe?g|png|gif|js|css|woff2)$">
                ExpiresActive On
                ExpiresDefault "access plus 30 days"
            </FilesMatch>
        </IfModule>

        # SSL/TLS Configuration
        SSLEngine on

        SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem
    </VirtualHost>

    # modern configuration
    SSLProtocol             all -SSLv3 -TLSv1 -TLSv1.1
    SSLCipherSuite          ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
    SSLHonorCipherOrder     on
    SSLSessionTickets       on

    # Enable HSTS
    Header always set Strict-Transport-Security "max-age=31536000"

    SSLUseStapling On
    SSLStaplingCache "shmcb:logs/ssl_stapling(32768)"

</IfModule>
```
