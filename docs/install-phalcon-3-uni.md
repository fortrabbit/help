---

template:         article
reviewed:         2017-06-16
title:            Install Phalcon 3
naviTitle:        Phalcon
lead:             Looking for sPHPeed? Phalcon is a web framework delivered as C extension providing high performance and low resource consumption. Here you learn how to best getting started with Phalcon 3 on fortrabbit.
group:            Install_guides
stack:            uni
proLink:          install-phalcon-3-pro
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

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine. You can skip the following two parts when you have chosen Phalcon in the [Software Preset](app#toc-software-preset) when creating your App.

### Root path

Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### Phalcon extension

In the Dashboard with your App: enable the **phalcon** PHP extension.

<div markdown="1" data-user="known">
[Edit the PHP settings for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/apps/{{app-name}}/settings)
</div>




## Quick start

### Install it locally

Phalcon is unlike any other framework. You first need to get Phalcon running locally. That's the hard part:

1. [Install the C-extension](https://docs.phalconphp.com/en/latest/reference/install.html)
2. [Download the Phalcon developer tools](https://docs.phalconphp.com/en/latest/reference/tools.html)
3. [Create a Phalcon project](https://docs.phalconphp.com/en/latest/reference/tools.html#generating-a-project-skeleton)
4. [Check the installation](https://docs.phalconphp.com/en/latest/reference/tutorial.html#checking-your-installation)

### Deploy to fortrabbit

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
$ git push -u fortrabbit master

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

Use App [environment variables](env-vars) to attain database login details at runtime. Open configuration file `app/config/config.php` and modify it directly after the `defined` statements like so:

```php
// ^^^ defined statements

return new \Phalcon\Config([
    'database'    => [
        'adapter'  => 'Mysql',
        'host'     => getenv('MYSQL_HOST') ?: 'localhost',
        'username' => getenv('MYSQL_USER') ?: 'your-local-db-user',
        'password' => getenv('MYSQL_PASSWORD') ?: 'your-local-db-password',
        'dbname'   => getenv('MYSQL_DATABASE') ?: 'your-local-db-name',
        'charset'  => 'utf8'
    ],
    'application' => [
        // do not change these contents
    ]
]);
```

#### MySQL access from local

Please see the [MySQL article](/mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.


### Updating Phalcon on fortrabbit

There might be a new version of the Phalcon extension available. At a certain point when software versions are becoming outdated (EoL) we will force these updates. We recommend to that before to leverage new features and improved speed.

Within the fortrabbit Dashboard: Go to the PHP Settings of your App. Check if it uses the latest PHP version and also enable the latest Phalcon version.

* Phalcon2 is available for PHP 5.X and PHP 7.X
* Phalcon3 is available for PHP 7.X
