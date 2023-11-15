---

template:    article
reviewed:    2023-11-15 15:55:23
title:       Redirects with .htaccess
naviTitle:   Redirects
lead:        'The most common use case for `.htaccess` is to re-write URLs with `mod_rewrite`. You can direct requests to a subdirectory, or specific domain, prettify URLs by omitting file endings, force https and much more.'
group:       htaccess
stack:       all
order:       5

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip


---

## Redirect all requests to https

**Force https!** There is no need for your application to be reached over a non-secure connection. Use `.htaccess` to redirect all `http://` requests over to `https://`. This is how:

```plain
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Proto} !=https
RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L,N]
```

**Order matters. This rule should be one of first rules, so right on top of your htaccess file** Please also note the X-Header part. Other code snippets you find elsewhere might not work here. This is because we are running our Apache behind a set of load-balancers. They are performing the HTTPS encryption and not Apache. Many CMS and frameworks are already offering convenient settings and configurations for this.

## Redirect all requests to the primary domain

Once you've added a [custom domain](/domains) you may want to prevent requests to your [App URL](/app#toc-app-url). The example below shows how to set up a domain based redirect in your `.htaccess` file.

```plain
# From App URL to your domain
RewriteEngine On
RewriteCond %{HTTP_HOST} ^.*\.frb\.io$ [NC]
RewriteRule .* https://www.your-domain.example%{REQUEST_URI} [r=301,L,N]
```

A CMS will have it's own options for that. If you don't use a redirect, make sure to at least set the "canonical URL" to be on your primary custom domain so that that search engines know that there is only one content.

## Force HTTPS for future visits with HSTS

This goes one step further than just forwarding requests, it tells the browser to not ever again accept any connection in the future on not secured domain to your App. In your `.htaccess` file you can add this line (in addition to the rewrite rule above):

```plain
<IfModule mod_headers.c>
Header always set Strict-Transport-Security "max-age=31536000"
</IfModule>
```

This will make your browser remember to always use the secured version of your App. It makes use of the "[HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)" policy and improves security by eliminating the risks of man-in-the-middle TLS-protocol-downgrade attacks. Be careful: setting this header will tell the browser never (or that is: until max-age) to use `http://` again. So if you later on decide to serve (parts of) your site using no encryption, all those clients (browsers) which saw the header will not comply and keep using `https://`.
