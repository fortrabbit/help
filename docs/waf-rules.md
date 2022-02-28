---

template:      article
reviewed:      2022-03-01
naviTitle:     WAF rules
title:         Web Application Firewall
excerpt:       Protect your App against common attacks and harmful requests
group:         platform
stack:         all
dontList:      true
dontIndex:     true
---


## Problem

Your App is exposed to the World Wide Web to give your users access. Unfortunately not every request is made by friendly humans. In fact there are bots, crawlers and attackers that scan your site for vulnerabilities. Even if they are not successful those requests are harmful because they eat resources which are meant for your users.

## Solution

Instead of a full-fledged Web Application Firewall we block most common automated request patterns.

## Basic WAF rules

By default "Basic WAF rules" are enabled, you can disable them in the dashboard. 

### Blocked files

* common dotfiles
* autodiscover.xml
* wallet.dat
* wlwmanifest.xml
* xmlrpc.php

### Blocked file extensions

* .asp
* .aspx
* .cgi
* .sql
* .swf


## Write your own rules using .htaccess

Intentionally we don't block every possible attack we've seen in the past. There are rules that would affect you but anybody else.
Use an `.htaccess` file to write your own rules. Below you can find some examples: 

### Non-WordPress sites should block all common WordPress files:

```plain
<Location ~ "(wp-login\.php|wp-comments-post\.php)$">
    Order allow,deny
    Deny from all
</Location>
```

### PMA

```plain
<Location ~ "(pma\.php|phpmyadmin\.php)$">
    Order allow,deny
    Deny from all
</Location>
```

### XXX YYY ZZZ

```plain
<Location ~ "(xxx\.php|yyy\.php|zzzz\.php)$">
    Order allow,deny
    Deny from all
</Location>
```

### WordPress sites should limit WP Admin to an IP if possible:

```plain
<Files wp-login.php>
    Order deny,allow
    Deny from all
    Allow from <YOUR IP>
</Files>
```
