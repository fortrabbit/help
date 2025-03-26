---

template:         article
reviewed:         2025-03-26 16:04:32
title:            Install Laravel
naviTitle:        Laravel
lead:             Laravel is the most PHPopular framework. Learn how to install and tune Laravel on fortrabbit.
group:            Install_guides

websiteLink:      http://laravel.com
websiteLinkText:  laravel.com
category:         framework
image:            laravel-black-new.svg
version:          12.x
stack:            all
supportLevel:     a

otherVersions:
    5: install-laravel-5-uni
    8: install-laravel-8
    9: install-laravel-9
    10: install-laravel-10

keywords:
    - php
    - install
    - laravel

rank: 80

---

## Get ready

Please make sure to have followed our [get ready guide](/get-ready) before starting here.

## Quick start

This is the fastest way to start with a fresh installation. Please scroll below for [migrating an existing Laravel](#toc-setup-for-an-existing-code-base). Execute the following in your terminal **on your local machine**:

```bash
# 1. Use Composer to create a local Laravel project named like your App
$ composer create-project laravel/laravel --prefer-dist {{app-name}}
# this installs Laravel with Composer locally and will take a while

# 2. Change into the folder
$ cd {{app-name}}

# 3. Initialize a local Git repo
$ git init

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Push changes to fortrabbit
$ git push -u fortrabbit main
# this installs Laravel with Composer on remote and takes a while

# the next deployments will be much faster
# 8. Push from now on
$ git push
```

If you are using the Laravel 11 default driver for sessions, which is `database`, you will also need to migrate the database upon deployment. The easiest way to do this is by adding this to your `composer.json`:

```json
"scripts": {  
    "post-install-cmd": [
        "@php artisan migrate --no-interaction --force"
    ],
}
```

With that in place, any time you deploy your code, database changes will be applied immediately. If no database changes are required, nothing happens, so it is safe to run all the time. Just make sure to test your upgrades and migrations locally first.

**Got an error?** Please see the [access troubleshooting](/access-methods). **Did it work?** Cool, when this is done, you can visit your App URL in the browser to see the Laravel welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

## Setup

**Don't stop with a plain vanilla installation. Make it yours!** Check out the following topics if you have an existing Laravel installation or if you would like to set up Laravel so that you can run it in a local development environment as well as in your fortrabbit App:


### Setup for an existing code base

You can also push your existing Laravel installation to fortrabbit. If you are already using Git, you can add fortrabbit as an additional remote, as described [above](#toc-quick-start) under point 6. When moving from another host to fortrabbit, please also read our [migration guide](/migrating).

### MySQL configuration

If you have chosen Laravel in the [Software Preset](app#toc-software-preset) when creating your App, we will automatically populate the "right" environment variables for the MySQL connection. So **you don't need to set anything**! Just keep `config/database.php` as it is.

The all CAPITAL configs in the `config/database.php` file will be replaced by the contents of the environment variables. For your local development setup you can populate the `.env` file with your local database credentials. See our [ENV var article](/env-vars) as well.

#### MySQL configuration with App secrets

Beside using ENV vars to configure your MySQL connection, you can also use fortrabbit App secrets ([see the article](secrets) for more info). Here is an example for `config/database.php`:

```php
// locally: use standard settings
$mysql = [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
];


// on fortrabbit: construct credentials from App secrets
if (getenv('APP_SECRETS')) {
    $secrets = json_decode(file_get_contents(getenv('APP_SECRETS')), true);
    $mysql = [
        'driver'    => 'mysql',
        'host'      => $secrets['MYSQL']['HOST'],
        'port'      => $secrets['MYSQL']['PORT'],
        'database'  => $secrets['MYSQL']['DATABASE'],
        'username'  => $secrets['MYSQL']['USER'],
        'password'  => $secrets['MYSQL']['PASSWORD'],
        'charset'   => 'utf8',
        'collation' => 'utf8_unicode_ci',
        'prefix'    => '',
    ];
}

return [
    'default'     => env('DB_CONNECTION', 'mysql'),
    'connections' => [
        'mysql' => $mysql,
    ],
    'migrations'  => 'migrations',
    // possible other code …
];
```

This configuration contains environment detection, so the App can run on your local machine with your local database, as well as on fortrabbit. You can also use App secrets to store custom third party access credentials.

## Database import and export

There are various use cases for exporting and importing databases. Likely you want to:

1. Export the database from your old web hosting
2. Export your local database to import it to the fortrabbit database
3. Export the remote database from fortrabbit to bring your local installation up-to-date

Read on in the [MySQL article](/mysql) on how to do that and other tasks.

## Working with artisan migrate

If you are using the Universal Stack, you can log in to [SSH](ssh-uni) and execute `artisan` on the App (note that you'll need to call `artisan` via PHP: more on that [here](/quirks#toc-need-to-call-via-php-interpreter)):

```bash
# login and execute
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
$ php artisan migrate --force
```

If you are using the Universal or the Professional Stack, you can [execute remote commands via SSH](/remote-ssh-execution-pro). For example:

```bash
# remote execution
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php artisan migrate --force'
```

**Note**: If `APP_ENV` is set to `production` - which is the default - the `--force` flag for migrate commands will override the confirmation prompt. 

You can also add the migrate command to your `composer.json` to have it run automatically every time you push changes ([see above](#toc-quick-start)).

## Logging

### Logging for Universal Apps

You can access your logs via SSH or SFTP. Laravel, per default, stores it's log files in `storage/logs`. You can download them via SFTP from that folder. If you need to "tail" your logs live, you can:

```bash
# login via SSH
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com

# tail the logs
$ tail -f storage/logs/laravel.log
```

### Logging for Professional Apps

By default Laravel writes all logs to `storage/logs/`. Since you don't have [direct file access](/app-pro#toc-ephemeral-storage), you need to configure Laravel to write to the PHP `error_log` method instead.

Laravel's `logging.php` config allows you to define various log channels. Make sure to add the `errorlog` channel to the `stack` or simply set the default channel via ENV var:

```
LOG_CHANNEL=errorlog
```

You can now use our regular [log access](logging-pro) to view the stream. 

[Flare](https://flareapp.io/) (no business association) is an error tracker for Laravel, which provides a searchable log archive and SMS, Email & Slack notifications.

## User sessions

There are various session drivers available in Laravel: see a [full list here](https://laravel.com/docs/11.x/session#configuration). Read further to see which ones are most suitable for your App. Whichever driver you end up using, you will need to specify it in the environment variables. Add a new ENV var `SESSION_DRIVER` in the Dashboard and give it the appropriate value.

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

### User sessions for Universal Apps

Since Universal Apps have persistent storage, you are able to use the default `file` driver for sessions. You can of course also use the other options specified in the [Laravel session docs](https://laravel.com/docs/11.x/session#configuration), though please note that we do not support Redis or Memcached out of the box. If you would like to use Redis, please see [our section on Redis below](#toc-working-with-redis). If you would like to use Memcached, consider switching to a Professional App. 

By default, Laravel 11 uses the `database` driver for session management. Just make sure you have run the default migrations and you'll be good to go.

### User sessions for Professional Apps

If you are on the Professional Stack, since you might have multiple Nodes and no persistent shared storage, you can not rely on the default Laravel `file` session driver. One solution to this issue is to use a cache-based store like [Redis](#toc-working-with-redis) or Memcached. See [our instructions below](#toc-setting-up-memcache-professional-stack-) for setting up the fortrabbit Memcache component. Don't forget to specify `memcached` as the `SESSION_DRIVER` environment variable. 

Note that, since Memcache is an in-memory store which does not persist its data over server reboots, for long-lived sessions the `database` driver should be preferred (see the previous section for details). 

## Queueing

### Queueing for Universal Apps

The Universal Stack does not support long running processes like `php artisan queue:work`.
Please see below for details on how to implement them in the Professional Stack.

### Queueing for Professional Apps

Laravel supports multiple queue drivers. One which can be used with fortrabbit out of the box is `database`, which simply uses your database connection as a queue. As of Laravel 11, the migration to create the jobs table is included by default, so as long as you have run your migrations upon installation, you're all set.

That's great for small use-cases and tinkering, but if your App handles very many queue messages you should consider [Redis](#toc-working-with-redis).

Once you've decided the queue you want to use, just open `config/queue.php` and set `default` to either `redis`, `database`, `sqs` - or even better: set the `QUEUE_CONNECTION` [environment variable](env-vars) accordingly in the Dashboard.

To run `php artisan queue:work` in the background, spin up a new [Worker](worker) and define the artisan command as a **Nonstop Job**.

**NB**: Laravel offers two commands to process queues: `queue:work` and `queue:listen`. We recommend using `queue:work`, and *not* using `queue:listen`. This is because the `queue:listen` command boots the Laravel framework for each iteration, whereas `queue:work` boots the framework once and runs as a daemon. Using `queue:work` offers high memory and performance gains in comparison with `queue:listen`.

## Setting up Object Storage (Professional Stack)

fortrabbit Professional Apps have an [ephemeral storage](/app-pro#toc-ephemeral-storage). If you require persistent storage for user uploads or any other runtime data your App creates (including assets like CSS and JS), you can use our [Object Storage Component](/object-storage). Once you have booked the Component in the Dashboard the credentials will become available via the [App secrets](/secrets). Using our `object-storage` driver reduces the configuration effort to a minimum:

```bash
composer require fortrabbit/laravel-object-storage
```

To make your App access the Object Storage, open up `config/filesystems.php` and modify it as follows:

```php
return [
    'default' => env('FILESYSTEM_DISK', 'local'),
    'cloud'   => env('FILESYSTEM_CLOUD', 's3'),
    'disks'   => [
            's3' => [
                'driver' => 'object-storage'
                // no further settings required
            ],
            // other disk …
    ]
];
```

If you want to use the Object Storage with your fortrabbit App and local storage with your local development setup then replace the "default" value in `filesystems.php` as well. Set `FILESYSTEM_DISK` in your local `.env` file to the value `local` and the [environment variable](/env-vars) in the Dashboard to the value `s3`. Also see the [README](https://github.com/fortrabbit/laravel-object-storage) of the repo.

## Using Vite

As of version 9, [Vite](https://vitejs.dev) replaces Mix as Laravel's default asset manager. Since fortrabbit does not provide a Node.js service, as with Mix you will need to compile your assets for production locally before deploying them to you App.

To get started, install Vite and the Laravel plugin:

```bash
npm install
```

After that, during local development, you can start the Vite dev server with

```bash
npm run dev
```

Your development asset files should be found in the locations specified in the `vite.config.js` file (by default 'resources/css/app.css' and 'resources/js/app.js').

When you are ready to deploy your assets, you must first build them for production locally with

```bash
npm run build
```

This will compile the assets and place them in a `build` folder in the `public` directory. This folder is by default excluded from the repo in `.gitignore`.

For more details on using Vite, see [Laravel's docs](https://laravel.com/docs/11.x/vite).

### Deploying assets with Universal Apps

We recommend using `rsync`. The one-liner below works with the most common scenarios, but feel free to adjust it to your needs if your setup differs:

```bash
# Rsync command to sync the assets in your /public folder 
rsync -av ./public/build/ {{app-name}}@deploy.{{region}}.frbit.com:./public/build/
```

For your convenience you can define rsync command `npm run deploy-assets`. Example of `package.json`:

```json
{
  "scripts": {
    "deploy-assets": "rsync -av ...",
   }
}
```

### Deploying assets with Professional Apps

You can export your minified assets to the [Object Storage](object-storage) with the `vite-plugin-s3`:

```bash
npm install fortrabbit/vite-plugin-s3
```

Now, to get your App's Object Storage details, enter this in the terminal:

```bash
ssh {{app-name}}@deploy.{{region}}.frbit.com secrets OBJECT_STORAGE
```

Then put the values which this command outputs in your `.env` file and prefix the keys with `VITE_OBJECT_STORAGE_`. In your `vite.config.js`, load the plugin and configure it with the env vars:

```js
import { defineConfig, loadEnv } from 'vite';
import laravel from 'laravel-vite-plugin';
import viteS3 from 'vite-plugin-s3';

export default defineConfig(({ mode }) => {
    const env = loadEnv(mode, process.cwd());
    return {
        plugins: [
            laravel({
                input: ['resources/css/app.css', 'resources/js/app.js'],
                refresh: true,
            }),
            viteS3({
                s3Options: {
                    accessKeyId: env.VITE_OBJECT_STORAGE_KEY,
                    secretAccessKey: env.VITE_OBJECT_STORAGE_SECRET,
                    region: env.VITE_OBJECT_STORAGE_REGION,
                    endpoint: env.VITE_OBJECT_STORAGE_SERVER,
                    signatureVersion: 'v2',
                },
                s3UploadOptions: {
                    Bucket: env.VITE_OBJECT_STORAGE_BUCKET,
                },
            }),
        ],
    };
});
```

Back in your terminal:

```bash
npm run build
```

Bear in mind that you need to [tell your source code](https://help.fortrabbit.com/object-storage#toc-http-access) to look for the minified CSS & JS files on the offshore Object Storage.

Another option might be to combine fortrabbit with GitHub Actions so you can have builds running over at GitHub and deploy everything along with artefacts afterwards. See our [blog post](https://blog.fortrabbit.com/how-to-use-github-actions).

### Migrating

If you want to migrate an existing project from Mix to Vite, Laravel has a [guide for that](https://github.com/laravel/vite-plugin/blob/main/UPGRADE.md#migrating-from-laravel-mix-to-vite). And if you've changed your mind, it has another guide for [migrating back from Vite to Mix](https://github.com/laravel/vite-plugin/blob/main/UPGRADE.md#migrating-from-vite-to-laravel-mix).


## Working with Redis

Redis can be used in Laravel as a cache or a queue or both. fortrabbit does not offer a Redis service of its own. To use Redis, you will need to book an externally-hosted Redis service. We have an article for [Redis Labs](/redis-cloud). 

1. On the fortrabbit side, turn on the Redis extension in the Dashboard under App > Settings > PHP.
2. Then setup an Account with a Redis provider. 
3. Then configure the redis database connection in `config/database.php`:

```php
// locally: use standards
$redis = [
    'host'     => env('REDIS_HOST', 'localhost'),
    'password' => env('REDIS_PASSWORD', null),
    'port'     => env('REDIS_PORT', 6379),
    'database' => 0,
];

// on fortrabbit: construct credentials from App secrets
if (getenv('APP_SECRETS')) {
    $secrets = json_decode(file_get_contents(getenv('APP_SECRETS')), true);
    $redis = [
        'host'     => $secrets['CUSTOM']['REDIS_HOST'],
        'port'     => $secrets['CUSTOM']['REDIS_PORT'],
        'password' => $secrets['CUSTOM']['REDIS_PASSWORD']
        'persistent' => 1
    ];
}

return [
    // other code …
    'redis' => [
        'cluster' => false,
        'default' => $redis
    ],
    // other code …
];
```

If you plan on using Redis as a cache, then open `config/cache.php` and set the `CACHE_DRIVER` [environment variable](env-vars) to `redis` in the Dashboard).

## Setting up Memcache (Professional Stack)

Memcache is a data store that can be used for caching and session storage. You can enable the [Memcache](memcache-pro) component in the Dashboard. Then you can use the [App Secrets](secrets) to get your credentials. Modify the `memcached` connection in your `config/cache.php` like so:

```php
// locally: use standard settings
$servers = [[
    'host' => env('MEMCACHED_HOST', '127.0.0.1'),
    'port' => env('MEMCACHED_PORT', 11211),
    'weight' => 100,
]];

// on fortrabbit: construct credentials from App secrets
if (getenv('APP_SECRETS')) {
    $secrets = json_decode(file_get_contents(getenv('APP_SECRETS')), true);
    $servers = [[
        'host' => $secrets['MEMCACHE']['HOST1'],
        'port' => $secrets['MEMCACHE']['PORT1'],
        'weight' => 100,
    ]];
    if ($secrets['MEMCACHE']['COUNT'] > 1) {
        $servers []= [
            'host' => $secrets['MEMCACHE']['HOST2'],
            'port' => $secrets['MEMCACHE']['PORT2'],
            'weight' => 100,
        ];
    }
}

if (extension_loaded('memcached')) {
    $timeout_ms = 50;
    $options = [
      // Assure that dead servers are properly removed and ...
      \Memcached::OPT_REMOVE_FAILED_SERVERS => true,
      
      // ... retried after a short while (here: 2 seconds)
      \Memcached::OPT_RETRY_TIMEOUT         => 2,
      
      // KETAMA must be enabled so that replication can be used
      \Memcached::OPT_LIBKETAMA_COMPATIBLE  => true,
      
      // Replicate the data, write it to both memcached servers   
      \Memcached::OPT_NUMBER_OF_REPLICAS    => 1,
      
      // Those values assure that a dead (due to increased latency or
      // really unresponsive) memcached server is dropped fast
      \Memcached::OPT_POLL_TIMEOUT          => $timeout_ms,        // milliseconds
      \Memcached::OPT_SEND_TIMEOUT          => $timeout_ms * 1000, // microseconds
      \Memcached::OPT_RECV_TIMEOUT          => $timeout_ms * 1000, // microseconds
      \Memcached::OPT_CONNECT_TIMEOUT       => $timeout_ms,        // milliseconds
      
      // Further performance tuning
      \Memcached::OPT_NO_BLOCK              => true,
    ];
}    

return [
    // other code …
    'stores' => [
        // other code …
        'memcached' => [
            'driver'        => 'memcached',
            'persistent_id' => env('MEMCACHED_PERSISTENT_ID'),
            'options'       => $options ?? [],
            'servers'       => $servers,
        ],
        // other code …
    ],
    // other code …
];
```

In addition, set the `CACHE_DRIVER` [environment variable](env-vars) so that you can use `memcached` in your production App on fortrabbit. If you don't have memcached on your local machine, set the driver to `file` or `database` via `.env`.


## Scheduling

The [Laravel scheduler](https://laravel.com/docs/11.x/scheduling) is not supported with the Universal Stack by design. The minimum time frame for standard cron jobs is 10 minutes here, but the Laravel scheduler requires a 1 minute schedule. Use the [Professional Stack](/app-pro) in combination with the [Workers Component](/worker-pro). That way your cron jobs will be outsourced to background processes. 

## Using artisan down (Professional Stack)

`artisan down` generates the file `storage/framework/down`, which is then checked from your App's HTTP kernel with the `CheckForMaintenanceMode` middleware. Modifying files via [SSH remote execution](remote-ssh-execution-pro) only affects the deployment Node, not your live App. Any file changes via SSH remote exec do not affect your App.

There are at least two options to do this:

1. Add `artisan down` as a `post-install-cmd` script in `composer.json`, then `git push` (remove the command and push again to bring it back online)
2. Use a custom middleware and command which uses another source than a file, like `memcache` or `database`

## Using Laravel Envoy

Easy. Here is an `Envoy.blade.php` example:

```php
@servers(['fr' => '{{ssh-user}}@deploy.{{region}}.frbit.com'])

@task('ls', ['on' => 'fr'])
    ls -lha
@endtask

@task('migrate', ['on' => 'fr'])
    php artisan migrate
@endtask
```

Then execute locally:

```bash
$ envoy run ls
$ envoy run migrate
```

## Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but Laravel provides an API over the popular SwiftMailer library. The mail configuration file is `config/mail.php`, and contains options allowing you to change your SMTP host, port, and credentials, as well as set a global form address for all messages delivered by the library.
