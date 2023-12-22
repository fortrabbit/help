---

template:         article
reviewed:         2023-09-14 11:39:30
title:            Tune Craft CMS
naviTitle:        5. Tune Craft
lead:             Tips, tricks, best practices and advanced topics on how to run Craft CMS successfully on fortrabbit.
group:            craft
stack:            all
order:            7

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          4.0
supportLevel:     a

otherVersions:
    3 : craft-3

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

rank: 90

---

## Get ready

Make sure to have followed [our guides](/craft-start) so far. You should have already [installed Craft locally](craft-install-local), [configured](/craft-setup) and deployed it your fortrabbit App. This guide helps you with running, tuning and troubleshooting.

### Security key setup

The mandatory Craft CMS security key has to be shared among all environments. We recommend to use your local security key as the master key. When you used Composer to install, that key was shown at the end of the installation. If not, open your local (hidden) `.env` file from the root folder of your project and find a line that looks like this:

```dotenv
CRAFT_SECURITY_KEY=69UzZSEquw9E7RdCyRRTRb1lxe7h0EPd
```

It will contain a value if you have [installed Craft 4 correctly on your local machine](/craft-install-local). Copy that line. Go to the App's ENV vars settings in the Dashboard and paste that line. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

That ENV var is already set. Just replace it with your local one. Also see the [official Craft guide on that topic](https://docs.craftcms.com/v3/installation.html#step-3-set-a-security-key) to learn about the different ways to create the key.

### Multi-environment configuration

Craft embraces the idea of storing environment specific configurations in ENV vars. The database config ([config/db.php](https://github.com/craftcms/craft/blob/main/config/db.php)) is a good example of that.

Additionally, you can create groups in every config file. The top level array key maps with the `CRAFT_ENVIRONMENT` constant, which defaults to the `CRAFT_ENVIRONMENT` ENV var. With this flexible approach you decide which configurations are under version control to share with your team and which are not. We assume fortrabbit to be your production environment, so the `CRAFT_ENVIRONMENT` ENV var is set to `production` on remote and locally to `dev`.

```dotenv
# Local environment ENV 
CRAFT_ENVIRONMENT=dev
```

### Domain setup

Your fortrabbit App comes with a predefined App Name and a URL like `{{app-name}}.frb.io` — which is good for testing. At some point you will very likely add your own domains. For general information on how to add domains to your fortrabbit App, please see our [domains article](/domains). For Craft CMS be sure to have set your domain's root path to the `/web` folder.

When installing Craft locally, it will ask you which domain you want to use. This local domain differs form the one you want to use in production. That's why it is stored in the `PRIMARY_SITE_URL` ENV var. The installer creates this ENV var in the .env file. In your fortrabbit production environment you may need to set `PRIMARY_SITE_URL` using the ENV var settings of your App. 

Craft CMS uses `@web` alias internally when it deals with HTTP requests. Unless you define it, Craft will try to figure it out based on HTTP headers - which is not always possible. It's good practise to define it explicitly using `PRIMARY_SITE_URL` ENV var.   

```php
return [
    // Recommended aliases in config/general.php
     'aliases' => [
          '@web' => \craft\helpers\App::env('PRIMARY_SITE_URL') ?: '/',
          '@webroot' => dirname(__DIR__) . '/web'
     ],
];
```

You can also add multiple domains. From the fortrabbit side, just point them to the App's root path, configure routing and display of contents within Craft CMS and/or use additional [htaccess rules](/htaccess).

#### Multi site domain settings

For multi-site setups, you may need to hard-code the domains for each environment. Here is an example of that:

```php
return [
    // fortrabbit (multi site)
    'production' => [
        'siteUrl' => [
            'en' => '//www.site.com',
            'nl' => '//www.site.nl',
        ],
    ],
    // local (multi site)
    'dev' => [
        'siteUrl' => [
            'en' => '//en.site.dev',
            'nl' => '//nl.site.dev',
        ],
    ],
];
```

#### Craft CMS on HTTPS

fortrabbit will provide Let's Encrypt TLS certificates for all domains; please see our [HTTPS article](/https) for more on that.

#### Using .htaccess for redirects and to force https

Craft CMS comes with a [predefined `.htaccess` file](https://github.com/craftcms/craft/blob/main/web/.htaccess) that lives inside the `web` folder, which is the root path. You can extend that with your own rules, like forwarding all requests to https or disabling access on the App URL. Please see our [.htaccess article](/htaccess) for examples.

### X-Powered-By headers

You probably think it's a good idea to disable headers that expose which PHP version and CMS you use. And we think so too!

However, internally we analyse this header to determine if it is a static or dynamic PHP response. With this information we generate two different metrics for the dashboard: PHP requests and Static requests.
In `config/general.php` you can disable the header with `'sendPoweredByHeader' => false,` (default: true). This is not required, since we strip all `X-Powered-By` headers eventually.

### Using project config

Craft will now always create multiple project config files in the `/config/project` directory.

With the [fortrabbit/craft-auto-migrate](https://github.com/fortrabbit/craft-auto-migrate) package, Project Config changes are applied automatically every time you `git push` changes.

### Enabling dev mode

Sometime while developing you might want to see some error output directly on your browser screen. That's what the `devMode` flag is for. See the [Craft docs](https://craftcms.com/knowledge-base/what-dev-mode-does) for more details.

### Don't allow updates in production

Craft CMS has the option to run updates directly from the Craft Control Panel. A client or editor might be tempted to use that update button in production. This is not a good idea. On fortrabbit Git is a [one-way street](/deployment-methods-uni#toc-git-works-only-one-way), so any changes on the App itself can not easily be ported back to the local development environment. Also `composer update` might be triggered on the App and that can lead to performance problems.

So it's better to prevent the shiny "update" button from showing up at all. You can do that in your Craft configuration in the general settings with the `allowUpdates` flag. Also see the [official guide](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-allowupdates).

### Don't allow admin changes in production

We highly recommend not making any admin changes to your production environment (fortrabbit) at all. Only do editorial changes. Have one single source of truth and only one direction (up). Please also see the official [Craft CMS docs](https://craftcms.com/docs/3.x/config/config-settings.html#allowadminchanges) on that. 

Otherwise you can run into trouble: Imagine you make changes to the database structure, by adding tables for example, changing the field layout in production. Now you don't have the changes locally. But you deploy other new changes from local to production. This might overwrite the `project.yml` in production and therefore will roll back your changes. Your local development should be your master in applying design and functional changes.

### Change the control panel URL

"Security through obscurity" is a widely discussed concept. We suggest to obscure the control panel URL of your Craft installation, just because you can. Do so with the `cpTrigger` setting. If you don't set this value it defaults to `admin`. The example below sets it to `godmode`, choose anything you like.

```php
return [
    'cpTrigger' => 'godmode'
];
```

### MySQL table prefixes

If your local Craft installation contains a table prefix, the one on the fortrabbit App should be the same. You can set the table prefix, locally in your `.env` file, and on fortrabbit with the App's [ENV vars](/#toc-craft-config-example) like so:

```dotenv
# Example Table prefix
CRAFT_DB_TABLE_PREFIX=craft_
```

## Manually setting ENV vars

Our [Software Preset](/app#toc-software-preset) will populate the ENV vars on fortrabbit for you (see [here](/env-vars) for more on ENV vars). So if you didn't choose the Craft preset when creating the App, or you are just curious how this works, or your ENV vars have been deleted accidentally, here is what will be set:

### Craft ENV vars on fortrabbit

```osterei32
CRAFT_DB_DRIVER=mysql
CRAFT_DB_PASSWORD=${MYSQL_PASSWORD}
CRAFT_DB_DSN=mysql:host=${MYSQL_HOST};port=3306;dbname=${MYSQL_DATABASE}
CRAFT_ENVIRONMENT=production
CRAFT_SECURITY_KEY=LongRandomString
```

## Using Craft CMS plugins

We highly recommend to not install plugins directly on your production App. We advise installing plugins locally first, and then deploying to get them installed on the App as well.

### Installing Craft CMS plugins

You have two options to install plugins:

#### A) Install Craft plugins from the control panel

This method let's you conveniently discover, install and also "activate" (pay for) the plugins right in place.
 
1. Visit the control panel of your local Craft installation
2. Visit the plugin store
3. Search, install and activate plugins

Plugins installed via the plugin store are "pinned" with `composer.json` and must also be updated through the same mechanisms. Please also see our [updating instructions](/craft-update).


#### B) Install Craft plugins via command line

This is a more advanced method to install plugins directly from the command line:

```bash
# 1. Add the plugin to Composer:
$ composer require {vendor/package-name}

# 2. Install the plugin via Craft CLI
$ ./craft plugin/install {plugin-handle}
```

Plugins installed that way can be updated with `composer update` later on. Please also see our updating instructions.

Bonus Tip: There is a Yii extension to install, uninstall, enable and disable plugins from the command line: [craft-plugin-commands](https://github.com/ostark/craft-plugin-commands).

### Using image tuning plugins

Image uploads to Craft are usually getting processed by ImageMagick. Image transformations happen when original images are sized down into several delivery formats. That happens during upload for display in the Craft Control Panel but also in your templates when you want to show those images to the users. Nowadays with image media queries like `srcset` one needs not only two sizes (thumbnail and large) but all kind of different sizes for different device resolutions. Please keep in mind that image transforms are "expensive" in terms of CPU utilization. Best don't overdo it. Use only as much as you'll need. Maybe four sizes per image are enough, for the imager plugin that can be four steps.

By the way: The image format `webp` is supported with fortrabbit's imageMagick version. Also check out our [application design article](/app-design) on website performance best practices. 

#### jpegoptim and company

[Some people suggest](https://nystudio107.com/blog/creating-optimized-images-in-craft-cms) further optimizing images with jpegoptim or optipng. These tools are not available here on fortrabbit, see [here why](/quirks#toc-no-root-shell). But there are some good alternatives. 

#### Using a third party image service

We advise using dedicated specialized third party image optimization services, like imgix, tinypng, kraken to do the job best.

## Craft CLI

Craft CMS comes with a built in command line interface which can be called from the console. You can also run it one the App (Universal Apps only) itself like so:

```
# Call Craft CLI via php
php craft
```

Note that you'll need to call `craft` via PHP, more on that [here](/quirks#toc-need-to-call-via-php-interpreter). 

Not all Craft CLI commands are safe to run in production. Our recommendations:

* `setup/*` — Don't, intended to run locally only
* `update/*` — Don't, if you deploy using git or using project config
* `clear-caches/*` — Don't, unless you have a good reason
* `resave/*` — Sometimes needed
* `project-config/sync` — Only needed if it's not part of your deployment
* `migrate/all` — Only needed if it's not part of your deployment
* `backup/db` — Useful to make a database backup. Can harm performance
* `restore/db` — Useful if you need to revert the DB to a previous state

## Webpack and Node build stuff

It is currently not possible to run Node.js tasks on the App or during deployment. If you are building CSS and JS from source, you will need to do that in your local development environment and deploy separately or via an external service (check our [blog post on GitHub Actions](https://blog.fortrabbit.com/how-to-use-github-actions)).

## Logging

There are two kind of `5xx` errors you can see on fortrabbit with Craft CMS: "Service Unavailable" by Craft CMS, or an error page rendered by fortrabbit. In the first case, at least something is working as Craft rendered the error. In the second case an exception is thrown before PHP reached the Craft CMS part.

Craft CMS is piping the PHP errors to its own location, located here:

```storage/logs/web-YYYY-MM-DD.log```

You can use [SFTP](/sftp-uni) or maybe better [SSH](/ssh-uni) to analyze the PHP error logs. Most likely you will find information on where the script has crashed and stopped. Also see our [log article](/logging-uni) for more details.

To access [php_error logs](/logging-pro#toc-log-access) on the Professional Stack you need to adjust Craft's log target in your `config/app.php` file. 

```php 
<?php

use craft\helpers\App;
use Monolog\Logger;
use Monolog\Handler\ErrorLogHandler;

return [
    'id' => App::env('CRAFT_APP_ID') ?: 'CraftCMS',
    'components' => [
        'log' => [
            'targets' => [
                [
                    'class' => \samdark\log\PsrTarget::class,
                    'except' => ['yii\web\HttpException:40*', 'yii\i18n\PhpMessageSource:*'],
                    'logVars' => [],
                    'logger' => (new Logger('app'))->pushHandler(new ErrorLogHandler()),
                    'addTimestampToContext' => true,
                ]
            ]
        ]
    ]
];
```

## Cache, Sessions and Mutex on the Pro Stack

On the [Pro Stack](/app-pro) in multi Node environments (see [terminology](/terminology)) you can not rely on the file based cache or session storage. Store this data in Memcached (a key-value storage) which is accessible from all Nodes instead - also see our [Memcache article](/memcache-pro). 

We have developed a composer package that integrates Craft with Memcache. With that no further configuration is required: you just need to pull it in to your `composer.json` like so:

```bash
$ composer require fortrabbit/yii-memcached
```

## Queue jobs

Craft CMS uses queue jobs for long-running tasks. By default these jobs are processed by a web based ajax call which blocks PHP processes that are meant for site delivery.
In the worst case scenario the queue runner consumes all PHP resources. Luckily there are more robust solutions available. [Andrew Welch](https://nystudio107.com/blog/robust-queue-job-handling-in-craft-cms) explains it in detail. Here is the TLDR:

### Use the Async Queue plugin

For most queue workloads the `ostark/craft-async-queue` plugin mitigates the problems described above. There is no further configuration required, just install it like any other plugin. See the [guide on Github](https://github.com/ostark/craft-async-queue) to understand what it does.  

### Offload to Worker job

On the [Pro Stack](/app-pro) you can run long running processes in a isolated environment without hurting site delivery.
You need to enable the [Worker](/worker-pro) component and set up a "Nonstop Job" with this Craft command: `php craft queue/listen -v`. To disable the default queue runner, `runQueueAutomatically` must be disabled in `config/general.php`.  

## Sending mail

You can not use sendmail on fortrabbit - see our [quirks article](/quirks#toc-mailing). Craft CMS allows your to configure sending mail via SMTP. It includes options to set your SMTP host, port, and credentials. You can use environment variables there as well. It's also possible to do that configuration within the Craft control panel.

Our recommendation: Use an external third party transactional mail provider for that. Pixel & Tonic (Craft CMS creators) maintain a [plugin for Postmark](https://plugins.craftcms.com/postmark). With that plugin installed you can easily set it up.

## Licensing Craft CMS

Craft CMS is not all free. To enable the good parts you need to obtain a licence from Pixel & Tonic (the folks building Craft CMS). 

A Craft CMS license is limited to a single domain, which means you can only access the Craft CP with one domain - otherwise you'll see a warning. You might have used the fortrabbit App URL for development and you might have used that to connect your Craft CMS licence with. You can change the domain of a Craft licence as well, if for instance you started with our App URL but now want to use your own domain with your Craft ID — over at https://id.craftcms.com/.

The App URL `{{app-name}}.frb.io` is already whitelisted as a testing URL, thanks to Pixel & Tonic.

## Headless Craft

By default HTTP responses with content type `text/html`, `text/css` and `text/javascript` are gzipped. When you use Craft in headless mode as a GraphQL or REST API, the content type is application/json. In the article about [GZIP compression](/gzip-compression) you learn how to enable it for other content types.

## Craft storage folder

We found a couple of Craft CMS installations blowing up the storage folder. This is often related to plugins and configuration. Some search engine bot might try to crawl all pages, causing a faceted search plugin to create gigabytes of template fragments. Some other plugin may just write very verbose logs. Note that we have a cron to keep the Craft CMS storage folder clean. Best, make sure to configure your website to not cause such issues, beside the web storage, such issues have impact on performance.

## Troubleshooting

### You see a Max Execution Time warning

The Craft CMS Control Panel has a system report that will check for technical requirements. Our default setting for `max_execution_time` is 60 seconds. The Control Panel will complain that: "Craft requires a minimum PHP max execution time of 120 seconds ... "

The truth is, only heavy tasks like updates, backups and queue execution in the Craft Control Panel may take longer than 60 seconds. For site delivery a short `max_execution_time` is beneficial to prevent blocking PHP resources. No HTTP response should take longer than a few seconds, actually.

Please see our [App design article](/app-design#toc-reduce-the-max-execution-time) for more details on the matter.


### You see a "Service Unavailable" or 503 message

What to do when your Craft CMS throws an "Service Unavailable" message on the screen instead of rendering your website? Don't be afraid, you can likely solve that yourself. Especially during setup it's likely that some tiny config is still missing or wrong. Check the logs.


#### Common errors with initial setup

Here are some common errors, the cause of 90% of failing Craft CMS installations here:

* **Mismatching Security Key** — Make sure to have the same key with your local development environment and on your fortrabbit App; see our [security key setup guide](craft-setup#toc-security-key).
* **Wrong database configuration** - Make sure your fortrabbit database is using the ENV vars provided by the fortrabbit Dashboard to connect to the fortrabbit database in production. Leave your `.env` file at home as it will be ignored anyway. See our [environment variables article](/env-vars) as well. 
* **Missing table prefix** - You might have a table prefix locally, some users do this locally to have multiple installations in one big database. To distinguish between them you can set up a table prefix. You can do this with an environment variable: locally you set that with your `.env` file, in fortrabbit via the Dashboard.
* **Missing .htaccess file** — Commonly happens with SFTP upload. The `.htaccess` file is hidden, make sure to copy it over as well.


#### Temporarily turning on/off dev mode

Your fortrabbit App is set to be your production environment per default. So when an runtime exception occurs, you will presented with the very basic "Service Unavailable" error screen. This is to prevent visitors of your website from seeing any information about the system and the errors. One thing you can do, is temporarily setting DevMode to true, so that you can see the error output printed on screen. We advise against doing it, unless it is a non-production environment. 


### You see a 504 message

That's a timeout issue. Commonly this happens when Craft is busy processing the queue or doing too many image transformations at once (see above please). 



### Large assets upload problems

Most likely this is a setting within Craft CMS itself. The setting you are looking for is `maxuploadfilesize` and its default is 16MB, please see the official [Craft Docs on that](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-maxuploadfilesize). This can also be caused by the `post_max_size`, `memory_limit`, `upload_max_filesize` or `max_execution_time` settings, which you can configure in the Dashboard, but by default those are OK. If that still doesn't help, check the [logs](/logging-uni) to see if you can find some meaningful errors.
