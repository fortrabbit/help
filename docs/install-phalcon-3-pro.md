---

template:         article
reviewed:         2022-03-18
title:            Install Phalcon 3
naviTitle:        Phalcon
lead:             Looking for sPHPeed? Phalcon is a web framework delivered as C extension providing high performance and low resource consumption. Here you learn how to best getting started with Phalcon 3 on fortrabbit.
group:            Install_guides
stack:            pro
uniLink:          install-phalcon-3-uni
deprecated:       yes
dontList:         true

websiteLink:      https://phalconphp.com/en/?utm_source=fortrabbit
websiteLinkText:  phalconphp.com
category:         framework
image:            phalcon-mark.png
version:          3.1.2
supportLevel:     c


keywords:
    - phalconphp
    - framework
    - php extension
    - c extension
    - hhvm

---


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine. You can skip the following two parts when you have chosen Phalcon in the Software Preset when creating your App.

### Root path

Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### Phalcon extension

Go to the Dashboard and enable the **phalcon** PHP extension.

<div markdown="1" data-user="known">
[Edit the PHP settings for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/apps/{{app-name}}/settings)
</div>




## Quick start

Phalcon is unlike any other framework. You first need to get Phalcon running locally. That's the hard part to run it locally:

1. [Install the C-extension](https://docs.phalconphp.com/en/latest/reference/install.html)
2. [Download the Phalcon developer tools](https://docs.phalconphp.com/en/latest/reference/tools.html)
3. [Create a Phalcon project](https://docs.phalconphp.com/en/latest/reference/tools.html#generating-a-project-skeleton)
4. [Check the installation](https://docs.phalconphp.com/en/latest/reference/tutorial.html#checking-your-installation)

Once that gives you a green light, continue by executing the following in your local terminal to start from scratch with a fresh new Phalcon installation on fortrabbit:

```bash
# 1. Create a local folder called like your App
$ mkdir {{app-name}}

# 2. Change into that folder
$ cd {{app-name}}

# 3. Initialize local Git repo
$ git init .

# 4. Make sure to keep the cache folder in Git
$ touch cache/.gitkeep

# 5. Add all files
$ git add -A

# 6. Commit files for the first time
$ git commit -m 'Initial'

# 7. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 8. Initial push and upstream
$ git push -u fortrabbit main

# 9. From there on only
$ git push
```

When it is done you can visit your App URL in the browser to see the Phalcon welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Advanced setup and migration

Until now this is a very vanilla Phalcon. Now, make it yours.

### Creating a project

Now you might want some stuff to happen there. See official guides to [get started with a Phalcon project](https://docs.phalconphp.com/en/latest/reference/tutorial.html#creating-a-project).


### MySQL

Use App [secrets](secrets) to attain database credentials. Open configuration file `app/config/config.php` and modify it directly after the `defined` statements like so:

```php
// ^^^ defined statements

// local MySQL credentials
$database = [
    'adapter'  => 'Mysql',
    'host'     => 'localhost',
    'username' => 'your-local-db-user',
    'password' => 'your-local-db-password',
    'dbname'   => 'your-local-db-name',
    'charset'  => 'utf8'
];

// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $database = [
        'adapter'  => 'Mysql',
        'host'     => $secrets['MYSQL']['HOST'],
        'username' => $secrets['MYSQL']['USER'],
        'password' => $secrets['MYSQL']['PASSWORD'],
        'dbname'   => $secrets['MYSQL']['DATABASE'],
        'charset'  => 'utf8',
    ];
}

return new \Phalcon\Config([
    'database'    => $database,
    'application' => [
        // do not change these contents
    ]
]);
```

#### MySQL access from local

Please see the [MySQL article](/mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.

### Memcache

Make sure you enabled the Memcache Component. Then you can use the [App Secrets](secrets) to attain your credentials.

#### Sessions

Open `app/config/services.php` and find the block starting with `$di->setShared('session', ...`. Replace it with the following:

```php
/**
 * Stash memcache servers into DI, so that they can be easily accessed by cache related services
 */
$di->setShared('memcache-servers', function() {
    if (isset($_SERVER['APP_SECRETS'])) {
        $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
        if (isset($secrets['MEMCACHE'])) {
            $servers = [];
            foreach (range(1, $secrets['MEMCACHE']['COUNT']) as $num) {
                $servers[]= [
                    'host' => $secrets['MEMCACHE']["HOST$num"],
                    'port' => $secrets['MEMCACHE']["PORT$num"],
                ];
            }
            return $servers;
        }
    }
    return [];
});

/**
 * Start the session the first time some component request the session service
 */
$di->setShared('session', function () {

    // on fortrabbit: construct credentials from App secrets
    if ($servers = $this->get('memcache-servers')) {
        $session = new \Phalcon\Session\Adapter\Libmemcached([
            'uniqueId'   => 'my-private-app',
            'servers'    => $servers,
            'lifetime'   => 3600,
            'client'     => [
                \Memcached::OPT_HASH      => \Memcached::HASH_MD5,
                \Memcached::OPT_PREFIX_KEY => 'prefix.',
            ],
            'prefix'     => 'sessions_'
        ]);
    }

    // locally: use the default session adapter
     else {
        $session = new SessionAdapter();
    }

    $session->start();

    return $session;
});
```

#### Backend Cache

Open up `app/config/services.php` again and add two new services:

```php
/**
 * Create the frontend cache the first time some component request the frontend cache service
 */
$di->setShared('frontend-cache', function() {
    return new \Phalcon\Cache\Frontend\Data([
        "lifetime" => 172800
    ]);
});

/**
 * Create the backend cache the first time some component request the backend cache service
 */
$di->setShared('backend-cache', function () {
    $frontendCache = $this->get('frontend-cache');

    // on fortrabbit: construct memcache credentials from App secrets
    if ($servers = $this->get('memcache-servers')) {
        $cache = new \Phalcon\Cache\Backend\Libmemcached($frontendCache, [
            'uniqueId'   => 'my-private-app',
            'servers'    => $servers,
            'lifetime'   => 3600,
            'client'     => [
                \Memcached::OPT_HASH      => \Memcached::HASH_MD5,
                \Memcached::OPT_PREFIX_KEY => 'prefix.',
            ],
            'prefix'     => 'cache_'
        ]);
    }

    // locally: use the file cache adapter
     else {
        $cache = new \Phalcon\Cache\Backend\File($frontendCache, [
            'cacheDir' => '../app/cache/'
        ]);
    }

    return $cache;
});
```

### Logging

To use the fortrabbit [log access](/logging-pro) the App needs to write all logs either to `STDERR` or use `error_log`. Phalcon supports the former. Open `app/config/services.php` and add a new logging service:

```php
/**
 * Create the logger facility the first time some component request the logger service
 */
$di->setShared('logger', function() {
    return new \Phalcon\Logger\Adapter\Stream("php://stderr");
});
```
