---

template:      article
reviewed:      2023-11-15 15:55:51
naviTitle:     WAF rules
title:         Web Application Firewall
excerpt:       Protect your App against common attacks and harmful requests
group:         platform
stack:         all
order:         10

---


## Problem

Your App is exposed to the World Wide Web to give your users access. Unfortunately not every request is made by friendly humans. In fact there are bots, crawlers and attackers that scan your site for vulnerabilities. Even if they are not successful those requests are harmful because they eat resources which are meant for your users.

## Solution

Instead of a full-fledged Web Application Firewall we block the most common automated request patterns.

## Basic WAF rules

By default "Basic WAF rules" are enabled; you can disable them in the Dashboard. 

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

Intentionally we don't block every possible attack we've seen in the past. There are rules which may be specific to your use case but which are not necessarily required for everyone. Use an `.htaccess` file to write your own rules.

See our [htaccess](/#htaccess) section for more.