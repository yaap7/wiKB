# Optimize web page loading

## Resources

* Google Chrome [LightHouse Extension](https://developers.google.com/web/tools/lighthouse/): Google Chrome extension which mesure a score for performance, accessibility, best practices, SEO, and progressive web app (PWA).

### Performance

* Google [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/): Web page load time with recommendations to improve your score
* [Web Page Test](https://www.webpagetest.org/): overall web page load time with scores, details, filmstrip view of your website during loading, etc.

### Security

* Qualys [TestSSL](https://www.ssllabs.com/ssltest/): One of the best SSL/TLS analyzer
* [SecurityHeaders](https://securityheaders.com/): Check the HTTP headers sent by your server

### Accessibility

A checklist: <https://design-accessible.fr/checklist>

### Protocols Support

* <https://ywo.fr/test_url.php>: my own tester (HTTP/2, Brotli, GZip, IPv6, etc. and links to other tools).

### Free illustrations

* <https://undraw.co/illustrations>

## Compress JPG and PNG Images

Use this script: [optimize-img](https://github.com/yaap7/miSCripts/blob/master/optimize-img)

Example:

``` bash
find . -type f -exec file -i {} \; | grep image | cut -d: -f 1 | xargs ~/tools/miSCripts/optimize-img
```

## Optimize JavaScript

### Use vanilla JavaScript instead of jQuery

* <https://flaviocopes.com/jquery/>
* <https://tobiasahlin.com/blog/move-from-jquery-to-vanilla-javascript/>

### Lightweight JavaScript frameworks

Use a light framework such as [svelte](https://svelte.dev/) instead of jQuery, the full Bootstrap, etc.
It embeds only the part you add at compile time, instead of loading the full framework at each visit of your website.

### Minify JavaScript

``` bash
uglifyjs -o agency.min.js agency.js
```

## Optimize CSS

### Lightweight CSS frameworks

Use a light framework such as [Knacss](https://www.knacss.com/) instead of a full bootstrap.

### Minify CSS files

``` bash
yui-compressor agency.used.css -o agency.used.min.css
```

## Apache Configuration

### Enable Caching in Browser

Enable the `expires` module:

``` bash
sudo a2enmod expires
sudo systemctl restart apache2
```

Add this configuration to the virtualhost:

``` xml
<IfModule mod_expires.c>
    <FilesMatch "\.(jpe?g|png|gif|js|css|woff2)$">
        ExpiresActive On
        ExpiresDefault "access plus 1 month"
    </FilesMatch>
</IfModule>
```

## WordPress extension

* [WP Fastest Cache](http://www.wpfastestcache.com/): optimize the cache policy (must enable it in the extension settings)
* [WebP Converter for Media](https://wordpress.org/plugins/webp-converter-for-media/): Automatically convert image to WebP format

## Enable CloudFlare

Using the (free) CloudFlare service will automatically cache your website.
To be continued.
