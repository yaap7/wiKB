# How To Make a Static Copy of a WordPress Instance

## WP2Static

This [WordPress plugin](https://fr.wordpress.org/plugins/static-html-output-plugin/) should do the trick but when I tried it, the default page was not the same as intended and the link menu were all pointing to the homepage.

Maybe it was because of the permalink settings was not correctly configured, but I abandonned this idea.

## wget

Back to basic, `wget` should do the trick!

``` bash
wget -q --mirror --convert-links --html-extension --adjust-extension --page-requisites https://example.com/
```
