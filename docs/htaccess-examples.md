---

template:    article
reviewed:    2023-11-15 15:54:00
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

### 1. Make sure environment variables are set

In the Dashboard with your fortrabbit App, make sure an environment variable key-value pair like this is present:

```.env
ENVIRONMENT=production
```

In some cases we have pre-populated that for you already. See also our [environment variables article](/env-vars) for more details on how environment variables are handled here.

### 2. Setup .htaccess files

Now in the local code base of your project, setup a custom `.htaccess` file for each environment in the web root of your project: 

* `.htaccess` in general and for local development
* `.htaccess_production` for your fortrabbit App

This can also be extended to more environments. All `.htaccess` files should be under version control with Git.

### 3. Configure Composer

Add this to the script part of your `composer.json` in your local code base:

```json
"scripts": {
    "post-install-cmd": [
      "@php -r \" define('HTSRC', 'web/.htaccess_' . getenv('ENVIRONMENT')); echo (file_exists(HTSRC) && copy(HTSRC, 'web/.htaccess')) ? HTSRC.' copied to web/.htaccess'.PHP_EOL : ''; \""
    ]
}
```

This Composer post install command will replace the contents of the `.htaccess` file with the contents of the file `.htaccess_production`, if that file is present and the environment variable `ENVIRONMENT` is set to `production`.

On fortrabbit, each time you deploy with Git (see our [Git deployment article](/git-deployment)), composer install runs automatically (see our [Composer article](/composer)).

You can extend the concept to have more stages of course (see our [multi staging article](/multi-staging)). To add custom rules for a staging environment, like http-auth (see [article](/http-auth)), create a matching `.htaccess_staging` file and set the `ENVIRONMENT` accordingly.

This also assumes that the root path of your App is `web`, which is true for Craft CMS. In the Composer example above, replace the `web` with the root path of your project, see an overview on different root paths [here](/app#toc-root-path).

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

This has to be a publicly accessible URL. You can do this with 4XX and 5XX errors. When using a framework or CMS likely the router will catch such errors and use PHP logic to resolve it, still some 5XX errors might appear before the program can execute the router.
