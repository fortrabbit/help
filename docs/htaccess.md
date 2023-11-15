---

template:    article
reviewed:    2023-11-15 15:54:11
title:       About .htaccess
naviTitle:   About .htaccess
lead:        Browsing the docs here you will find lot's of reference to a mysterious invisible file called ".htaccess". What's that about? How can you make use of it?
group:       htaccess
stack:       all
order:       1

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip


---


<!--

DO NOT USE THE .htaccess class for code blocks, it will break the $ from being copy/pasteable....

-->

`.htaccess` is a hidden file that usually lives in the web root folder of your code base. It enables altering the web server's configuration directives. `.htaccess` rules apply to all subdirectories. `.htaccess` is usually not excluded in `.gitignore` so it will be deployed alongside your code. Take care: htaccess is a sharp sword. With great power comes great responsibility.

## .htaccess on fortrabbit

Your fortrabbit Apps are running on the Apache web server. You can make use of `.htaccess`. Missing (remember it's hidden) and wrong `htaccess` directives are common issues. These sensitive defaults are set on the fortrabbit platform regarding .htaccess:

* Apache configuration syntax 2.2 and 2.4 are supported 
* GZIP compression is enabled per default for text based content types
* Access on all `.ht*` files is disabled, so nobody can read your .htaccess

## .htaccess and your framework or CMS

When you are using a framework or a CMS, chances are high, that you don't need to wrangle with `.htaccess` at all, as that comes built-in. Here are the most used ones:

* **Laravel** comes with a [boilerplate htaccess](https://github.com/laravel/laravel/blob/master/public/.htaccess) you can extend
* **Craft CMS** comes with a [boilerplate htaccess](https://github.com/craftcms/craft/blob/main/web/.htaccess) you can extend
* **WordPress** [manages htaccess](https://codex.wordpress.org/htaccess) for you

## htaccess topics

Browse our [.htaccess section](/#htaccess) for examples on how to do redirects (https and domain), secure your WordPress, how to control HTTP headers and more.
