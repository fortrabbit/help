---

template:         article
reviewed:         2022-03-18
title:            Install Symfony 3
naviTitle:        Symfony 3
lead:             Symfony has been around for some while — but it doesn't look old. Learn how to install and tune Symfony 2 or 3 on fortrabbit.

group:            Install_guides
dontList:         true
deprecated:       true
stack:            all

websiteLink:      http://symfony.com/?utm_source=fortrabbit
websiteLinkText:  symfony.com
category:         framework
image:            symfony-mark.png
version:          3.4.4

otherVersions:
  6 : install-symfony-6

---


## Get ready

We assume you've already created a New App and chose Symfony in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard). You should also have a [PHP development environment](/local-development) running on your local machine.

### Root path

If you did not choose Symfony stack when creating the App in the Dashboard at first, please set the following: Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **web**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


### ENV vars

Go to the Dashboard and add the following [environment variables](/env-vars) to your App:

```plain
SYMFONY_ENV=prod
SYMFONY__DATABASE__USER=${MYSQL_USER}
SYMFONY__DATABASE__PASSWORD=${MYSQL_PASSWORD}
SYMFONY__DATABASE__HOST=${MYSQL_HOST}
SYMFONY__DATABASE__PORT=${MYSQL_PORT}
SYMFONY__DATABASE__NAME=${MYSQL_DATABASE}
```

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>



## Quick start

For a new Symfony installation execute the commands following in your local terminal:

```bash
# 1. Use Composer to create a local Symfony project named like your App
$ composer create-project symfony/framework-standard-edition {{app-name}} "3.4.*"

# 2. Change into the folder
$ cd {{app-name}}

# 3. Initialize a local Git repo
$ git init .

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Push changes to fortrabbit
$ git push -u fortrabbit main

# 8. After the first push you only need
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool! This first push can take a bit, since all the Composer packages need to be installed. When the push is done you can visit your App URL in the browser and see the Symfony welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Advanced setup and migration

Until now this is a vanilla Symfony. It needs some more tinkering to make it yours.

### MySQL

The MySQL access details are available via [environment variables](env-vars). If you have chosen Symfony in the Stack chooser when creating the App, we will automatically create `SYMFONY__DATABASE__*` environment for you and you can use them as following (if you haven't chosen the right stack, please [add those Symfony env vars manually, as shown above](#toc-env-vars)):

Open `app/config/parameters.yml.dist` and modify all `database_*` parameters:

```yaml
parameters:
    # ... keep above
    database_host: %database.host%
    database_port: %database.port%
    database_name: %database.name%
    database_user: %database.user%
    database_password: %database.password%
    # ... keep below
```


### Use app_dev.php

If you want to use the development environment, you must modify `web/app_dev.php`. A simple example would be to replace the block, responding with a 403 like so:

```
if (isset($_SERVER['APP_NAME']) && $_SERVER['APP_NAME'] === '{{app-name}}') {
    // allow
} elseif (isset($_SERVER['HTTP_CLIENT_IP'])
    || isset($_SERVER['HTTP_X_FORWARDED_FOR'])
    || !(in_array(@$_SERVER['REMOTE_ADDR'], array('127.0.0.1', 'fe80::1', '::1')) || php_sapi_name() === 'cli-server')
) {
    header('HTTP/1.0 403 Forbidden');
    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
}
```

This way you can easily decide per App whether you want to allow the dev mode or not.


### Logging

In Universal stack, you can access all log files your App writes on the file system. If you want to use live logging in [Universal stack](logging-uni#toc-live-log-access) or in [Professional stack](logging-pro), then you should configure Symfony to use `error_log`. Modify the `app/config/config_prod.yml` file:

``` yaml
monolog:
    # ..
    handlers:
        # ..
        nested:
            type:  error_log
            # ..
```

### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use the `SwiftmailerBundle` and configure it in your `app/config/config.yml` file. Make sure you set the [charset](encoding) to UTF-8:

```php
Swift_Preferences::getInstance()->setCharset('UTF-8');
```

### Cache

In Universal stack you can use the default file cache. In Professional stack, when you deploy your app, the old code is removed and the new one is deployed.
If you need at some point to store custom information in the cache (through cache pools), you then need to avoid filesystem (because of the way the Professional stack works, with its distributed nodes): instead, use an adapter amongst `doctrine`, `redis` or `memcached`.
