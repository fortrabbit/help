---

template:         article
reviewed:         2023-12-21 11:16:56
title:            Install Symfony
naviTitle:        Symfony
lead:             Symfony has been around for some while — but it doesn't look old. Learn how to install and tune Symfony 7 on fortrabbit.

group:            Install_guides
stack:            all

websiteLink:      https://symfony.com
websiteLinkText:  symfony.com
category:         framework
image:            symfony-mark.png
version:          "7"
supportLevel:     a

otherVersions:
  3 : install-symfony-3


---


## Get ready

We assume you've already created a new App and chose Symfony in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard). You should also have a [PHP development environment](/local-development) running on your local machine. If you are using Symfony 4, 5 or 6, you can safely follow this guide, given Symfony 7 did not break compatibility changes.

### Root path

If you did not choose Symfony when creating the App in the Dashboard at first, please set the following: Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### Env vars

Symfony [environments](https://symfony.com/doc/current/configuration.html#configuration-based-on-environment-variables) are controlled by the `APP_ENV` env var. You can even use env vars in the .yml configurations now. To get you started quickly, we provide you with the following env vars by default if you chose the Symfony preset:

```osterei32
APP_DEBUG=0
APP_ENV=prod
APP_SECRET=ClickToGenerate
DATABASE_URL=mysql://${MYSQL_USER}:${MYSQL_PASSWORD}@${MYSQL_HOST}/${MYSQL_DATABASE}
```

You can view and edit the env vars in the Dashboard under your App settings. You can also define your own key-value pairs and use them in your configuration files.

<div markdown="1" data-user="known">
[Go to env vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

## Quick start

It is assumed that you already have a Symfony project installed and running locally. This can be an existing project or a new one, see the [official download page](https://symfony.com/download) for instructions for your local operating system.

Given that fortrabbit uses Apache as a web server, consider fetching the default `.htaccess` file for Symfony, via Flex (since this is from the "contrib" repository, you will be asked to confirm the installation. Do it by pressing `y`):

```bash
$ composer require symfony/apache-pack
```

After running the command, the `.htaccess` should be copied to your /public folder. Open the file and replace `Options +FollowSymlinks` with `Options +SymLinksIfOwnerMatch`.

If your project is not under Git version control yet, follow these steps to be prepared for your first `git push`.

```bash
# 1. Initialize a local Git repo
$ git init .

# 2. Add all files
$ git add -A

# 3. Commit files for the first time
$ git commit -m 'Initial'

# 4. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 5. Initial push and set upstream
$ git push -u fortrabbit main
# Lots of output

# 6. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool! This first push can take a bit, since all the Composer packages need to be installed. When the first push is done you can visit your App URL in the browser and see the Symfony welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

## MySQL

Until now you just deployed some code. If you want to use Doctrine and MySql it requires some more tinkering to make it yours.

### Configuration

The `DATABASE_URL` env var stores all DB access information already (see [above](#toc-env-vars)). You just need to use it in `doctrine.yaml` like so:  

```yaml
doctrine:
    dbal:
        # All in one single line (default)
        url: '%env(DATABASE_URL)%'

```

### Doctrine & Symfony console

Once Doctrine is configured and the changes are deployed, you may want to run migrations and maybe load fixtures. You can login to your App via [ssh](ssh), or instead just fire single commands like so:

```bash
# Doctrine
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php bin/console doctrine:migrations:migrate'
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php bin/console doctrine:fixtures:load'

# cache clear
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php bin/console cache:clear'
```

You can also add this migrate command to your `composer.json` to have it run automatically every time you push changes.

```json
"scripts": {
    "post-install-cmd": [
        "php bin/console doctrine:migrations:migrate --no-interaction",
    ],
}
```

With that in place, any time you deploy your code, database changes will be applied immediately. If no database changes are required, nothing happens, so it is safe to run all the time. Just make sure to test your upgrades and migrations locally first.

## Deploying compiled JavaScript & CSS

There currently is no possibility to use Node.js with our services. The build process to compile JS and CSS needs to run locally - whether with [AssetMapper](https://symfony.com/doc/current/frontend/asset_mapper.html), [Encore](https://symfony.com/doc/current/frontend/encore/index.html), [Webpack](https://www.npmjs.com/package/webpack), [Gulp](https://www.npmjs.com/package/gulp), [Grunt](https://www.npmjs.com/package/grunt), or the solution of your choice.

Compiled assets should not be under version control. So, instead of committing the build files to Git, you deploy them separately. 

Given the way our stacks are designed (Universal having persistent storage and Professional allowing horizontal scaling), deploying your assets will be done differently.

### Deploying compiled assets for Universal Apps

To deploy your compiled assets to your app, when using the Universal Stack, you can simply use rsync. It works great for this and it's easier than you might think:

```bash
# Deploy the build folder
$ rsync -av ./public/build/ {{app-name}}@deploy.{{region}}.frbit.com:~/public/build/
```

### Deploying compiled assets for Professional Apps

There is no persistent storage with the Professional Stack.  We provide Object Storage for this, see the [main article](/object-storage) on how to access it.

## Advanced configurations

Still reading? Let's go on:

### Logging

In the Universal Stack, you can access all log files your App writes on the file system. If you want to use live logging in the [Universal Stack](logging-uni#toc-live-log-access) or in the [Professional Stack](logging-pro), then you should configure Symfony to use `error_log`. After having installed the Monolog bundle (`composer require symfony/monolog-bundle`), modify the `config/packages/monolog.yaml` file:

``` yml
monolog:
    # ..
    handlers:
        # ..
        nested:
            type: error_log
            # ..
```

### Sending emails

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use `Swiftmailer`.

Symfony provides a Mailer component that makes things easy. To configure the way your emails are sent, set the `MAILER_DSN` environment variable.

More on this in the [official documentation](https://symfony.com/doc/current/mailer.html).

### Cache

In the Universal Stack you can use the default file cache. In the Professional Stack, when you deploy your app, the old code is removed and the new one is deployed.

If you need at some point to store custom information in the cache (through cache pools), you then need to avoid the filesystem (because of the way the Professional Stack works, with its distributed nodes): instead, choose an adapter from `doctrine`, `redis` or `memcached`.
