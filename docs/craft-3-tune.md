---

template:         article
reviewed:         2019-12-10
title:            Tune Craft CMS
naviTitle:        Tune Craft
lead:             Tips, tricks, best practices and advanced topics on how to run Craft CMS successfully on fortrabbit.
group:            craft
stack:            all
order:            20

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.3

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local), [configured](/craft-3-setup) and deployed it your fortrabbit App. This guide helps you with running, tuning and troubleshooting.


## Domain setup

Your fortrabbit App comes with a predefined App Name and an URL like `{{appname}}.frb.io` — which is good for testing. At some point you will very likely add your own domains. For general information on how to add domains to your fortrabbit App, please see our [domains article](/domains). For Craft CMS make sure to have set your domains root path to `/web` folder. 

Craft CMS usually plays well with any domain, as long as you have used the "@web" prefix in your settings and templates. 

You can also add multiple domains. From the fortrabbit side, just point them to the Apps root path, configure routing and display of contents withing Craft CMS and/or use additional [htaccess rules](/htaccess).


### Licensing Craft CMS

Craft CMS is not all free. To enable the good parts you need to obtain a licence  from Pixel & Tonic (the folks building Craft CMS). 

A Craft CMS license is limited to a single domain, which means you can only access the Craft CP with one domain - otherwise you'll see a warning. You might have used the fortrabbit App URL this for development and you might have used that to connect your Craft CMS licence with. You can change the domain of a Craft licence as well when for instance you have started with our App URL but now want to use your own domain with your Craft ID — over at https://id.craftcms.com/.


## Multi-environment configuration

Craft embraces the idea of storing environment specific configurations in ENV vars. The database config ([config/db.php](https://github.com/craftcms/craft/blob/master/config/db.php)) is a good example for that.

Additionally you can create groups in every config file. The top level array key maps with the `CRAFT_ENVIRONMENT` constant, which defaults to the `ENVIRONMENT` ENV var. With this flexible approach you decide which configurations are under version control to share it with your team and which are not.

We assume fortrabbit to be your production environment, so the `ENVIRONMENT` ENV var is set to `production` on remote and locally to `dev`.

```dotenv
# Local environment ENV setting
ENVIRONMENT=dev
```


### Craft CMS configuration details

```php
<?php
return [
    // Global settings
    '*' => [
        'useProjectConfigFile' => true,
        'cpTrigger'            => 'godmode',
        'userSessionDuration'  => 'P24H',
        'securityKey'          => getenv('SECURITY_KEY'),
        'siteUrl'              => getenv('SITE_URL') ?: '@web'
    ],    
    // fortrabbit
    'production' => [
        'devMode'      => false,
        'allowUpdates' => false
    ],
    // local
    'dev' => [
        'devMode' => true
    ]
];
```

Above is an extended example for `config/general.php`. It sets some useful shared settings, as well as some environment specific settings for production (fortrabbit) and development (locally). Here are the details on the settings:


### Project config

Craft 3.1 introduced the project config. It helps keeping track of config settings and changes outside of the database (separation of concerns). It's currently not enabled by default. You can enable it using the `useProjectConfigFile` setting. We highly recommend using it.


### Dev mode

Sometime while developing you might want to see some error output directly on your browser screen. That's what the `devMode` flag is for. See the [Craft docs](https://craftcms.com/support/dev-mode) for more details.


### Don't allow updates in production

Craft CMS has the option to run updates directly from the Craft Control Panel. A client or editor might be tempted to use that update button in production. This is not a good idea. On fortrabbit Git is a [one-way street](/deployment-methods-uni#toc-git-works-only-one-way), so any changes on the App itself can not easily be ported back to the local development environment. Also `composer update` might be triggered on the App and that can lead to performance problems.

So you better prevent the shiny "update" button from showing up at all. You can do that in your Craft configuration in the general settings with the `allowUpdates` flag. Also see the [official guide](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-allowupdates).


### Change the admin URL

"Security through obscurity" is a widely discussed concept. We suggest to obscurify the control panel URL of your Craft installation, just because you can. Do so with the `cpTrigger` setting. If you don't set this value it defaults to `admin`. The example above sets it to `godmode`, choose anything you like.


### Domain settings

Using `'siteUrl' => getenv('SITE_URL') ?: '@web'` like in the example above tells Craft CMS to use the `SITE_URL` ENV var or the `@web` fallback, which is a good default. 

#### Multi site domain settings

For multi site setups, you may need to hard-code the domains for each environment. Here is an example for that:

```php
<?php
return [

    // fortrabbit (multi site)
    'production' => [
        'siteUrl' => [
            'en' => '//www.site.com',
            'nl' => '//www.site.nl',
        ]
    ],
    // local (multi site)
    'dev' => [
        'siteUrl' => [
            'en' => '//en.site.dev',
            'nl' => '//nl.site.dev',
        ]
    ]
];
```


## Manually setting ENV vars

Our [Software Preset](/app#toc-software-preset) will populate the ENV vars on fortrabbit for you, see [here](/env-vars) for more on ENV vars. So when you have not chosen Craft while creating the App, or you are just curious how this works, or your ENV vars have been deleted accidentally, here is what will be set:

### Craft ENV vars on fortrabbit

```osterei32
DB_DATABASE=${MYSQL_DATABASE}
DB_DRIVER=mysql
DB_PASSWORD=${MYSQL_PASSWORD}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
ENVIRONMENT=production
SECURITY_KEY=LongRandomString
```

### MySQL table prefixes

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. You can set the table prefix, locally in your `.env` file and on fortrabbit with the App's [ENV vars](/#toc-craft-config-example) like so:

```dotenv
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

## Updating Craft CMS

We recommend to always use the latest version for security reasons. Mind that you are responsible for the software you write yourself and use. **Test updates locally first!** For production (your fortrabbit App) we have set the ENV "allowUpdates" to false (see above). That will make the update button not show up in the control panel, so that your client will not accidentally update. Depending on your deployment workflow — [Git](/craft-3-deploy-git) or [SFTP](/craft-3-upload-sftp) — there are two ways to update Craft:

### A. Update Craft with a Git workflow (recommended)

When you have used a Git to deploy Craft, your update workflow can looks like this: Use Composer from the command line to first update your local installation, then push the changes to trigger the update on remote. Run the following command in the terminal on your computer **locally**:

```bash
# Make sure to be in the projects root folder (locally)
$ composer update

# The output looks something like this:
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 0 installs, 9 updates, 0 removals
  - Updating symfony/process (v4.0.11 => v4.4.1): Downloading (100%)
  - Updating symfony/console (v4.0.11 => v4.4.1): Downloading (100%)
  - Updating craftcms/cms (3.0.11 => 3.3.5): Downloading (100%)
  [...]
  - Updating symfony/event-dispatcher (v4.0.11 => v4.4.1): Downloading (100%)
Writing lock file
Generating optimized autoload files
```

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App. During the [Git deployment](/git-deployment), `composer install` will run automatically. This way your local Composer changes get applied on the remote. Some plugins or the Craft core may include database migrations. Don't forget to run the following SSH remote execution command after the updated packages are deployed:

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft migrate/all"
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft project-config/sync"

```

To automate this task, we created a little package which triggers `migrate/all` and `project-config/sync` every time you push changes.

```bash
# Install the craft-auto-migrate package
$ composer require fortrabbit/craft-auto-migrate
```

With that in place, any time you deploy your code, database changes will be applied immediately. If no database changes are required, nothing happens, so it is safe to run all the time. Just make sure to test your upgrades and migrations locally first.


### B. Update Craft with a SFTP workflow

It's a bit more dangerous, updating Craft with a SFTP workflow. The goal is - again - to have your local Craft CMS and the one on fortrabbit in sync and to start locally first. Mind: An update will change the files, this might also trigger changes in the database structure. 

Locally: Use the update button in the control panel, or like above "composer update". This will also change your local database structure.

Then upload the file changes. `rsync` can help to sync only changed files, see our [rsync article](/rsync). Now your files on the fortrabbit are up-to-date, but the database structure might not. To run the database migrations on the fortrabbit App and thereby finish the installation, access the control panel on remote and hit the "Finish" button.


## Image tuning

Image uploads to Craft are usually getting processed by ImageMagick. [Some people suggest](https://nystudio107.com/blog/creating-optimized-images-in-craft-cms) to further optimize images with jpegoptim or optipng. These tools are not available here on fortrabbit, see [here why](/quirks#toc-no-root-shell). But there are some good alternatives. We evangelize to use dedicated specialized third party image optimization services, like imgix, tinypng, kraken or imageoptim to do the job best. These two Craft plugins are supporting external services:

* [Imager Craft](https://github.com/aelvan/Imager-Craft/)
* [Craft Imageoptimize](https://github.com/nystudio107/craft-imageoptimize)

Don't forget that this is only tuning — making images a little smaller. Also check out our [application design article](/app-design) on website performance best practices.

## Cache and sessions on the Professional Stack

On the [Pro Stack](/app-pro) in multi Node environments - see [terminology](/terminology) - you can not rely on the file based cache or session storage. Store this data in Memcache a key-value-storage which is accessible from all Nodes instead - also see [Memcache article](/memcache-pro). We have a custom extension developed. With that no further configuration is required, you just need to pull it in to your `composer.json` like so:

```bash
$ composer require fortrabbit/yii-memcached
```


## HTTPS

fortrabbit will provide Let's Encrypt TLS certificates for all domains, please see our [HTTPS article](/https) for more on that.

## htaccess

Craft CMS comes with a [predefined `.htaccess` file](https://github.com/craftcms/craft/blob/master/web/.htaccess) that lives inside the `web` folder, which is the root path. You can extend that with your own rules, like forwarding all requests to https or disabling access on the App URL. Please see our [.htaccess article](/htaccess) with examples.


When enabled, Craft CMS will create and update a file called `project.yml` containing Craft configuration. Please see the [Craft CMS docs article](https://docs.craftcms.com/v3/project-config.html#enabling-the-project-config-file) for more on it. 
With the `fortrabbit/craft-auto-migrate` package mentioned above, Project Config changes are applied automatically to your fortrabbit App. 


## Queue Jobs

Craft CMS uses queue jobs for long-running tasks. By default these jobs are processed by a web based ajax call which blocks PHP processes that are meant for site delivery.
In the worst case scenario the queue runner consumes all PHP resources. Luckily there are more robust solutions available. [Andrew Welch](https://nystudio107.com/blog/robust-queue-job-handling-in-craft-cms) explains it in detail, here is the TLDR:

### Async Queue Plugin

For most queue workloads the `ostark/craft-async-queue` plugin mitigates the problems described above. There is no further configuration required, just install it like any other plugin. See the [guide on Github](https://github.com/ostark/craft-async-queue) understand what it does.  

### Worker Job

On the [Pro Stack](/app-pro) you can run long running processes in a isolated environment without hurting site delivery.
You need to enable the [Worker](/worker-pro) component and set up a "Nonstop Job" with this Craft command: `craft queue/listen`. To disable the default queue runner, `runQueueAutomatically` must be disabled in `config/general.php`.  

## Troubleshooting

### You see a Max Execution Time warning

The Craft CMS Control Panel has a system report that will check for technical requirements. Our default setting for `max_execution_time` is 60 seconds. The Control Panel will complain that: "Craft requires a minimum PHP max execution time of 120 seconds ... "
The truth is, only heavy tasks like updates, backups and queue execution in the Craft Control Panel may take longer than 60 seconds. For site delivery a short `max_execution_time` is beneficial to prevent blocking PHP resources. No HTTP response should take longer than a few seconds, actually.
Please see our [App design article](/app-design#toc-reduce-the-max-execution-time) for more details on the matter.

### You see a "Service Unavailable" or 5xx message

What to do when your Craft CMS throws an "Service Unavailable" message on the screen instead of rendering your website? Don't be afraid, you can likely solve that yourself. Especially during setup it's likely that some tiny config is still missing or wrong.

### Common errors while initial setup

Here are some common errors, the cause of 90% of failing Craft CMS installations here:

* **Mismatching Security Key** — Make sure to have the same key with your local development environment and on your fortrabbit App
* **Wrong database configuration** - Make sure your fortrabbit database is using the ENV vars provided by the fortrabbit Dashboard to connect to the fortrabbit database in production. Leave your .env file at home as it will be ignored anyways.
* **Missing .htaccess file** — Commonly happens with SFTP upload, the `.htaccess` is hidden, make sure to copy it over as well

### Temporary turning on/off dev mode

Your fortrabbit App is set to be your production environment per default. So when an runtime exception occurs, you will represented with the very basic "Service Unavailable" error screen. That's a feature, as visitors of your website should not be able to get any information about the system and the errors. One thing you can do, is temporarily setting DevMode to true, so that you can see the error output printed on screen. So so in the fortrabbit Dashboard, with the App under ENV vars, set the "devMode" to true (or 1). Don't forget to turn this off, later on, as you do not want to expose your development settings to the world.


### Logging

There are two kind of 5xx errors you can see on fortrabbit with Craft CMS: The "Service Unavailable" by Craft CMS, or an error page rendered by fortrabbit. In the first case, at least something is working as Craft rendered the error, in the second case an exception is thrown before PHP reached the Craft CMS part.

Craft CMS is piping the PHP errors to it's own location, located here:

```storage/logs/web.log```

You can use [SFTP](/stfp-uni) or maybe better [SSH](/ssh-uni) to analyze the PHP error logs. Most likely you will find information on where the script has crashed and stopped. Also see our [log article](/logging-uni) for more details.

To access [php_error logs](/logging-pro#toc-log-access) on the Professional Stack you need to adjust Craft's log target in your `config/app.php` file. 

```php
<?php 
// app.php
return [
    'components' => [
        'log'    => [
            'targets' => [
                function () {
                    return new class() extends \craft\log\FileTarget
                    {
                        public function init()
                        {
                            $this->setLevels(
                                YII_DEBUG === true
                                    ? ['trace', 'warning', 'error']
                                    : ['warning', 'error']
                            );
                        }

                        public function export()
                        {
                            $text = implode(PHP_EOL, array_map([$this, 'formatMessage'], $this->messages));
                            // revert to php defaults & log
                            ini_set('error_log', 'syslog');
                            error_log($text . PHP_EOL);
                        }
                    };
                }
            ]
        ]
    ]
]; 
```

### Large assets upload problems

Most likely this is a setting within Craft CMS itself. The setting you are looking for is `maxuploadfilesize` and it's default is 16MB, please see the official [Craft Docs on that](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-maxuploadfilesize). This can also be caused by the `post_max_size`, `memory_limit`, `upload_max_filesize` or `max_execution_time` which you can set in the Dashboard, but by default those are OK. If that still doesn't help, check the [logs](/logging-uni) if you can find some meaningful errors.


### Sending mail

You can not use sendmail on fortrabbit - see our [quirks article](/quirks#toc-mailing). Craft CMS allows your to configure sending mail via SMTP. The mail configuration can be done in `project.yaml`. It includes options to set your SMTP host, port, and credentials. You can use environment variables there as well. It's also possible to do that configuration within the Craft control panel.
