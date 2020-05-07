# Optimize web page loading

## Resources

* Google Chrome [LightHouse Extension](https://developers.google.com/web/tools/lighthouse/): Google Chrome extension which mesure a score for performance, accessibility, best practices, SEO, and progressive web app

### Performance

* Google [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/): Web page load time with recommendations to improve your score
* [Web Page Test](https://www.webpagetest.org/): overall web page load time with scores, details, filmstrip view of your website during loading, etc.

### Security

* Qualys [TestSSL](https://www.ssllabs.com/ssltest/): One of the best SSL/TLS analyzer
* [SecurityHeaders](https://securityheaders.com/): Check the HTTP headers sent by your server

## Compress JPG and PNG Images

Install the dependencies: [jpegtran](https://github.com/mozilla/mozjpeg) and [pngquant](https://pngquant.org/).

Use this script: [optimize-img](https://github.com/yaap7/miSCripts/blob/master/optimize-img)

Example:

``` bash
find . -type f -exec file -i {} \; | grep image | cut -d: -f 1 | xargs ~/tools/miSCripts/optimize-img
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

Using the (free) CloudFlare service will automatically cache 
