---

template:         article
reviewed:         2020-03-27
title:            Install Symfony 5
naviTitle:        Symfony
lead:             Symfony has been around for some while — but it doesn't look old. Learn how to install and tune Symfony 5 on fortrabbit.

group:            Install_guides
stack:            uni
proLink:          install-symfony-5-pro

websiteLink:      https://symfony.com
websiteLinkText:  symfony.com
category:         framework
image:            symfony-mark.png
version:          "5 & 4"
supportLevel:     a

otherVersions:
  3 : install-symfony-3-uni
  4 : install-symfony-4-uni


---


## Get ready

We assume you've already created a new App and chose Symfony in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard). You should also have a [PHP development environment](/local-development) running on your local machine.


### A note on Symfony versions

Symfony 5 did not bring backward breaking changes. So you can safely follow this guide for Symfony 4 and Symfony 5.


### Root path

If you did not choose Symfony when creating the App in the Dashboard at first, please set the following: Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### ENV vars

Symfony 5 [environments](https://symfony.com/doc/current/configuration/environments.html#executing-an-application-in-different-environments) are controlled by the `APP_ENV` ENV var. You can even use ENV vars in the .yml configurations now. To get you started quickly, we provide you with the following ENV vars by default when you have chosen the Symfony from when creating the App:

```osterei32
APP_ENV=prod
APP_DEBUG=0
APP_SECRET=ClickToGenerate
DATABASE_URL=mysql://${MYSQL_USER}:${MYSQL_PASSWORD}@${MYSQL_HOST}/${MYSQL_DATABASE}
```

Within the Dashboard under your App settings you can modify the ENV vars. Modify `APP_DEBUG` or `APP_ENV` to change the behavior of your application. You can also define your own key-value-pairs and use them in your configuration files.

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>


## Quick start

We assume that you already have your [Symfony project locally](http://symfony.com/download).

Given that fortrabbit uses Apache as a web server, consider fetching the default .htaccess file for symfony, via flex (since this is from contrib repository, you will be asked to confirm the installation. Do it by pressing `y`):

```bash
$ composer require symfony/apache-pack
```

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
$ git push -u fortrabbit master
# Lots of output

# 6. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool! This first push can take a bit, since all the Composer packages need to be installed. When the first push is done you can visit your App URL in the browser and see the Symfony welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## MySQL

Until now you just deployed some code. If you want to use doctrine and Mysql it requires some more tinkering to make it yours.

### Configuration

The `DATABASE_URL` ENV var stores all DB access information already (see [above](#toc-env-vars)). You just need to use it in `doctrine.yaml` like so:  

```yaml
doctrine:
    dbal:
        # All in one single line (default)
        url: '%env(DATABASE_URL)%'

```

### Doctrine & symfony console

Once doctrine is configured and the changes are deployed, you may want to run migrations and maybe load fixtures. You can login via [ssh](ssh) in to your App, or instead just fire single commands like so:

```bash
# doctrine
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

## Webpack Encore

We assume you are using Encore to manage your CSS & JS assets and `yarn` is configured in the local environment already.

### Installation

Locally:

```
$ composer require symfony/webpack-encore-bundle
$ yarn install
```

### Configuration

In your `webpack.config.js` define a few basic options for your webpack installations:

```js
var Encore = require("@symfony/webpack-encore");

if (!Encore.isRuntimeEnvironmentConfigured()) {
    Encore.configureRuntimeEnvironment(process.env.NODE_ENV || "dev");
}

Encore.setOutputPath("public/build/")
    .setPublicPath("/build")
    .addEntry("app", "./assets/js/app.js")
    .enableSourceMaps(!Encore.isProduction())
    .enableVersioning(Encore.isProduction());

module.exports = Encore.getWebpackConfig();
```

This will generate a `build` directory in `public`, with a manifest.json and your your compiled assets. Symfony already knows where to look for files thanks to the `config/packages/assets.yaml` file.

### Deploying assets

Compiled assets should not be under version control. Installation of Encore via flex should have added `public/build` to your `.gitignore` already.
So, instead of committing the build files to Git, you deploy them separately. rsync works great for this and it's easier than you might think:

```bash
# Build production assets locally
$ yarn run encore production

# Deploy the build/prod folder
$ rsync -av ./public/build/ {{app-name}}@deploy.{{region}}.frbit.com:~/public/build/
```


## Advanced configurations

Still reading? Let's go on:

### Logging

You can access all log files your App writes on the file system. If you want to use [live logging](logging#toc-live-log-access), then you should configure Symfony to use `error_log`. After having installed monolog bundle (`composer require symfony/monolog-bundle`), modify the `config/packages/prod/monolog.yml` file:

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

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use the `Swiftmailer`.
Symfony provides a Mailer component that makes things easy. To configure the way your emails are sent, mind setting a `MAILER_DSN` environment variable.
More on this in the [official documentation](https://symfony.com/doc/current/mailer.html).
