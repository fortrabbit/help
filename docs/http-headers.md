---

template:    article
reviewed:    2023-11-15 15:54:07
title:       Control HTTP headers
naviTitle:   HTTP headers
lead:        'HTTP headers are part of an Hyper Text Transfer Protocol request and response. HTTP headers are defining the operating parameters of an HTTP transaction. Use htaccess to modify these.'
group:       htaccess
stack:       all

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip

rank:     3

---

## Cache-Control

Don't serve the same content to the same client twice! Control how the browser of the client caches results and files locally. This is especially useful for asset resources that don't change often. Caching reduces the number of request and the data transmitted. On the HTTP part of your App/website caching is achieved by using HTTP headers. You can control the caching in htaccess like so:

```plain
# Example to cache images and CSS files
# adjust and extend to your needs
<ifModule mod_headers.c>
  #  images expire after 1 month
  <filesMatch ".(gif|png|jpg|jpeg|webp|ico|pdf|svg|js)$">
    Header set Cache-Control "max-age=2592000"
  </filesMatch>
  # CSS expires after 1 day
  <filesMatch ".(css)$">
    Header set Cache-Control "max-age=86400"
  </filesMatch>
</ifModule>
```

If you are using a CMS this is usually already managed for you in PHP code. You can open the web inspector on your website, go to the network tab and check the headers for any images, to see if they already have cache headers.

Beware: Adding this also introduces the main problem of caching. If you later decide to change the image or css file, you must either wait the full cache time, or give the file a new name to make the browser retrieve it again. Otherwise it will use the old cached version.

## CORS headers

For "Cross-Site XMLHttpRequests" you'll need "Cross-origin resource sharing" or in short CORS headers. Those are mostly used in context of JavaScript AJAX requests across different domains. Like when `domain-a.com` loads a script from `domain-b.com`. Per default this is not possible for security reasons. But you can enable it for certain or even all origins:

```plain
# Access all areas (use carefully)
Header add Access-Control-Allow-Origin "*"
Header add Access-Control-Allow-Methods: "GET,POST,OPTIONS,DELETE,PUT"
```

Use this with care and only open what you really need. Reduce the risk of XSS. Also check out the new Content Security Policy (CSP) for more advanced control on what you allow and what not. This website is a good reference: [content-security-policy.com](https://content-security-policy.com/)