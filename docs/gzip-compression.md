---

template:    article
reviewed:    2025-07-09 14:41:04
title:       How to enable GZIP compression with htaccess
naviTitle:   GZIP compression
lead:        ''
group:       htaccess
stack:       all
order:       9

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip


---

HTTP responses are compressed if the `content-type` header indicates a text document, for example `text/css` or `text/html`. To enable GZIP compression for other HTTP responses, like your REST or GraphQL APIs, use the `AddOutputFilterByType` directive:

```plain
AddOutputFilterByType DEFLATE application/json
```

## 1. Make sure environment variables are set

In the Dashboard with your fortrabbit App, make sure an environment variable key-value pair like this is present:

```.env
ENVIRONMENT=production
```

In some cases we have pre-populated that for you already. See also our [environment variables article](/env-vars) for more details on how environment variables are handled here.

## 2. Setup .htaccess files

Now in the local code base of your project, setup a custom `.htaccess` file for each environment in the web root of your project: 

* `.htaccess` in general and for local development
* `.htaccess_production` for your fortrabbit App

This can also be extended to more environments. All `.htaccess` files should be under version control with Git.

## 3. Configure Composer 

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