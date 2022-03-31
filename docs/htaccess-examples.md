---

template:    article
reviewed:    2022-03-24 15:00:00
naviTitle:   .htaccess examples
title:       .htaccess by example
lead:        'Here some examples what .htaccess can be used for.'
group:       htaccess
stack:       all
order:       2

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip

---

## Tip: Comment your .htaccess file

Apache directives with regular expressions tend to look gibberish. Make sure to comment what you have been doing there so that "the later you" can understand what's going on.

## Non-WordPress: block all common WordPress files

```plain
<Location ~ "(wp-login\.php|wp-comments-post\.php)$">
    Order allow,deny
    Deny from all
</Location>
```

## Block specific file extensions

```plain
# Archives you probably don't want to expose
<FilesMatch "\.(bz2|gz|zip)$">
   Order allow,deny
   Deny from all
</FilesMatch>
```

## Environment detection

You might want some `.htaccess` rules only to be applied in a certain environment. A common use case is that is to forward all traffic to HTTPS on your fortrabbit App in production, but not in your local development environment. 

## Allow access only for you

You may want to prevent accessing your website for anyone except your company. If your company has a fixed IP, you can use `.htaccess` to only allow traffic from that IP or range of IPs.

```
### 1st deny all

ErrorDocument 403 "Not Allowed"
Deny from all

### 2nd Allow your IPs 

Allow from 11.11.11.11
Allow from 12.12.12.12
```

An alternative to this is to use a password check with [HTTP Auth article](/http-auth).

### WordPress: limit WP Admin to an IP if possible

```plain
<Files wp-login.php>
    Order deny,allow
    Deny from all
    Allow from <YOUR IP>
</Files>
```

## Custom error pages

You can define custom pages to make your error pages look more cool like so:

```htaccess
ErrorDocument 404 /404.html
```

This has to be a publically accessible URL. You can do this with 4XX and 5XX errors. When using a framework or CMS likely the router will catch such errors and use PHP logic to resolve it, still some 5XX errors might appear before the program can execute the router.
