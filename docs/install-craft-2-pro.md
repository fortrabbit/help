---

template:         article
reviewed:         2017-12-20
title:            Install Craft CMS 2 on fortrabbit
naviTitle:        Craft
group:            Install_guides
stack:            pro
uniLink:          install-craft-2-uni

websiteLink:      https://craftcms.com/?utm_source=fortrabbit
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          2.6
group:            Install_guides
deprecated:       yes
dontList:         yes

otherVersions:
    3 : craft-3-about

keywords:
    - craft
    - craftCMS
    - setup
    - install-guide

---


## Please note

**Craft 2 is deprecated.** Better not use it any more. It's end of life was/will be January 2020. See more details on the [official Craft CMS page](https://craftcms.com/guides/craft-cms-2-and-craft-commerce-1-end-of-life-information). We do have an [upgrade guide](/craft-2-3-upgrade).


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.

### Set the root path

If you haven't already (the Software Preset does that for you) - in the Dashboard: [Set the document root](/domains#toc-root-path) of your App's domains to `public`. This applies to all domains, either the App URL or your external domains.

### Set ENV vars

If you haven't already (the Software Preset does that for you) - in the Dashboard set the following [environment variables](env-vars):

```osterei32
CRAFT_DEBUG=0
CRAFT_CACHE=memcache
CRAFT_UPDATES=0
CRAFT_KEY=LongRandomString
```

## Install

If you are just testing then best use the [HappyLager Demo](https://github.com/pixelandtonic/HappyLager) and follow their [install guide](https://github.com/pixelandtonic/HappyLager#installation) to run it locally first. If you have a running (production) installation then you need to export its data and set up a local, working "clone" with which you can proceed.


## Configure the fortrabbit environment

Craft's native multi-environment configuration (also see our [multi-staging article](multi-staging)) options allow you to define configuration options based on the domain name. This is great, but there is a potential security flaw (when using Git based deployments) you should be aware of: You're hard-coding the configuration details of your production environment into your code, which means you will have sensitive information in your Git version history. Let's solve this.

Open up `craft/config/general.php` and replace all contents it like so:

```php
<?php

$protocol = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == 'on') ? 'https' : 'http';

return [
    'allowAutoUpdates'  => getenv('CRAFT_UPDATES') ?: true,
    'cacheMethod'       => getenv('CRAFT_CACHE') ?: 'file',
    'devMode'           => getenv('CRAFT_DEBUG') ?: false,
    'validationKey'     => getenv('CRAFT_KEY') ?: false,
    'siteUrl'           => $protocol . '://' . $_SERVER['HTTP_HOST'] . '/',
    'baseCpUrl'         => $protocol . '://' . $_SERVER['HTTP_HOST'] . '/',
    'environmentVariables' => [
        'assetsBaseUrl'  => '/assets',
        'assetsBasePath' => './assets',
    ]
];
```

## MySQL

Your App needs database access - when working local and on remote. We provide `MYSQL_...` environment variables automatically, so that you don't have to care. Open `craft/config/db.php` in your editor and modify it like the following:

```php
return [
	'server'      => getenv('MYSQL_HOST')     ?: 'localhost',
	'database'    => getenv('MYSQL_DATABASE') ?: 'local-db-name',
	'user'        => getenv('MYSQL_USER')     ?: 'local-db-user',
	'password'    => getenv('MYSQL_PASSWORD') ?: 'local-db-password',
	'tablePrefix' => 'craft',
];
```

### MySQL access from local

Please see the [MySQL article](mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.



## Object Storage

Since the Professional Stack does not support a [persistent storage](app-pro#toc-ephemeral-storage) you want to use the [Object Storage](object-storage) to save your uploads and static assets.

We've prepared a Craft plugin that acts as a drop-in replacement for the Amazon S3 asset source. [Download the plugin](https://github.com/fortrabbit/craft-s3-fortrabbit) from GitHub and **follow the setup instructions in the [README.md](https://github.com/fortrabbit/craft-s3-fortrabbit/blob/master/README.md)**. Once that's done, you can create your Object Storage asset source:

1. In the Craft admin: Go to Settings > Assets
2. Click on `New asset source` to create a new `AssetSource`
3. Give it a name like `Object Storage`
4. Set the Type to `Amazon S3`
4. Enter the Access Key ID and Secret Access Key
5. Click on `Refresh`
5. Select a `Bucket`
6. Enter a `Subfolder` (use the App name here)
7. Click `Save` in the upper right corner

You now need to move all the "local assets" to the newly created `Object Storage` asset source:

1. Go to Assets
2. Drag & drop assets to the `Object Storage` asset source (left side)

**Note**: Rinse and repeat with all your local asset sources! To make use of the cloud storage support of Craft you need a "Pro" license.

Now that is done you can safely remove the empty, local asset sources:

1. Got to Settings > Assets
2. Click the remove button for every `Local Folder` type asset source


### Cache & sessions

If you are just testing out Craft: make sure you use Development PHP plan with fortrabbit. With those you can set the `CRAFT_CACHE` env var to `file`, `db` or `apc` during your tinkering, for example:

```
CRAFT_CACHE=db
```

All fortrabbit production PHP plans run on multiple Nodes and require a cache which is accessible jointly - same goes for sessions, of course. This disqualifies `apc`, `db` and `file` and leaves you with `memcache` as the only option.

So make sure you have booked the [Memcache component](/memcache-pro) and then create the file `craft/config/memcache.php` with the following content:

```php
if (getenv('MEMCACHE_COUNT')) {

    $handlers = [];
    $servers  = [];
    foreach (range(1, getenv('MEMCACHE_COUNT')) as $num) {
        $handlers[] = getenv('MEMCACHE_HOST' . $num) . ':' . getenv('MEMCACHE_PORT' . $num);
        $servers[] = [
            'host'          => getenv('MEMCACHE_HOST' . $num),
            'port'          => getenv('MEMCACHE_PORT' . $num),
            'retryInterval' => 2,
            'status'        => true,
            'timeout'       => 2,
            'weight'        => 1,
        ];
    }

    // session config
    ini_set('session.save_handler', 'memcached');
    ini_set('session.save_path', implode(',', $handlers));
    
    if (getenv('MEMCACHE_COUNT') == 2) {
        ini_set('memcached.sess_number_of_replicas', 1);
        ini_set('memcached.sess_consistent_hash', 1);
        ini_set('memcached.sess_binary', 1);
    }

    // craft config
    return [
        'servers'      => $servers,
        'useMemcached' => true,
    ];
}    
    
```

### Database migration

In case you have a database you want to export/import: Database migration is straight forward: export the database of the local installation and import it to your fortrabbit App. You can use a [GUI](mysql#toc-mysql-guis) or the shell. From the shell you start out by [opening up a tunnel](mysql#toc-shell-tunnel-mysql) and then use `mysqldump` to export and `mysql` to import:

```bash
# on your local machine

# export from local
$ mysqldump database-name > database.sql

# import to fortrabbit using the tunnel
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < database.sql
```


## Deploy with Git

Now that you have the configuration done, let's get the code up. If your local Craft installation is not under Git version control already, then you do it now:

```
# Initialize Git
$ git init .

# Add your App's Git remote to your local repo:
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
```

Now open and modify the `.gitignore` file:

```bash
# Exclude misc project files
*.idea/*
*.log
*.DS_Store
*Thumbs.db
*.sass-cache/
node_modules
*config.codekit
*.sql

# Exclude uploads, runtime data and temp files, but keep the layout assets
/craft/storage/runtime/
/craft/storage/cache/
/craft/storage/logs/
/craft/storage/compiled_templates/
/public/assets/*
!/public/assets/css/*
!/public/assets/js/*
!/public/assets/fonts/*

# Exclude Composer's vendor folder
/vendor/
```

You're almost done with the basic setup. Last step is to deploy your code:

```
# 1. Add changes to Git
$ git add -A

# 2. Commit changes
$ git commit -m 'Init'

# 3. Initial push and upstream
$ git push -u fortrabbit master

# 4. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool, when this is done, you can visit your App URL in the browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Tuning


### Logging & debugging

Per default Craft writes all the logs to a file, which won't do. So we wrote a simple plugin, which writes all logs to STDERR instead. Then the logs are accessible via our standard [logging](logging-pro). The plugin can be found here: [github.com/ostark/craft-stderr-logger](https://github.com/ostark/craft-stderr-logger)


### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but Craft provides multiple alternative options. In the Craft admin: just go to Settings > Email, change the `Protocol` to `SMTP` or `Gmail` and enter the credentials of your mail service.
