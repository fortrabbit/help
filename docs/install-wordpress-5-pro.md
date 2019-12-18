---

template:         article
reviewed:         2018-12-23
title:            Install WordPress 5
naviTitle:        WordPress
lead:             WordPHPress is PHPowering much of the web. Learn here how to install and tune the popular blogging and CMS engine WordPress 5 on fortrabbit.
group:            Install_guides
workInProgress:   yes

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          5.0.2
stack:            pro
uniLink:          install-wordpress-5-uni
supportLevel:     b

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development), including a web server, a MySQL database, [Coomposer](/composer) and PHP obviously running on your local machine.

### Set the root path

If you haven't already (the [Software Preset](app#toc-software-preset) does that for you) — in the Dashboard: [Set the root path](/app#toc-root-path) of your App's domains to **web**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

<div markdown="1" data-user="known">
[Go to my ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>


## Install locally

WordPress itself has not really arrived in the new PHP age in terms of using the latest technologies and paradigms. [Bedrock](https://roots.io/bedrock/) is a WordPress boilerplate for usage in a modern 12-factor oriented infrastructure, such as fortrabbit. Let's go:

### Create WordPress locally with Bedrock

Issue the following commands in your local terminal:

```bash
# 1. Create a new local project folder, name the folder like your App
$ composer create-project roots/bedrock {{app-name}}

# 2. Go into that folder
$ cd {{app-name}}
```

### Configure your local installation

It's time to make some configurations on your local WordPress installation so that it can run locally now and later on fortrabbit — all with the same code base. Bedrock allows to use ENV vars to do this. As long as you have chosen WordPress in the Software chooser, the ENV vars for the fortrabbit App should be set already. So you now deal with the ENV vars for your local development. For the next steps, you might open your entire local {{app-name}} folder with your IDE or text editor.

Edit your local `.env` file for that. It's in the root folder of your project folder, and it's hidden. Please see our dedicated [ENV vars article](/env-vars) if you are unsure about that concept.

Populate the following configurations with what you have running locally — your local database connection and the address (vhost) of your local installation:

```env
DB_NAME=local_database_name
DB_USER=local_database_user
DB_PASSWORD=local_database_password
DB_HOST=localhost
WP_HOME=localhost
```

The actual data you have to enter there depends on your local development environment of course. Make sure to have created a local MySQL database for WordPress to use. Often the database user is called "root" and the database password empty. The database host might also be 127.0.0.1. `WP_HOME` here, is where your local WordPress website can be reached in the browser, that again depends on your [local development](/local-development) setup (MAMP, Valet, Vagrant …). Keep your local `.env` file open, you will need it for the next steps.

### Set keys and salts

WordPress will encrypt and decrypt your passwords with authentication unique keys and salts. So in order to be able to use the same database in production (with fortrabbit) and locally, those keys should be the same. 

We **recommend** to use the ones set with fortrabbit (setup by the [Software Preset](app#toc-software-preset)). Within the Dashboard of your App, under ENV vars, copy the environment vars: `AUTH_KEY`, `SECURE_AUTH_KEY`, `LOGGED_IN_KEY`, `NONCE_KEY`, `AUTH_SALT`, `SECURE_AUTH_SALT`, `LOGGED_IN_SALT`, `NONCE_SALT` and there contents (long cryptic strings). Paste those with your local `.env` configuration file. **Alternatively** you can also go the other way around and first create your keys locally and then copy those keys to the fortrabbit App in the Dashboard.

<div markdown="1" data-user="known">
[Go to my ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

Now, that should be all you'll need to set in your local `.env` file and we are ready to proceed.


### Run the installer

Now, it's time to really install WordPress on your local machine. That will populate the database with tables and also setup your admin user. To do so, start the installation steps by calling your local development address served by your local PHP server. When that is done, you should be able to browse your local WordPress site. Also you should be able to login the wp-admin area with your newly created admin user. 

If not: Make sure that your [local development](/local-development) is setup correctly and you have followed the guide so far without mistakes. Don't skip. It's crucial to setup your project locally.


## Deploy WordPress to fortrabbit

Now, you have WordPress running with Bedrock locally and already configured it to play nice with your fortrabbit App. Let's set it live on fortrabbit:

### MySQL sync

Next off, we want the remote database to know about our first local contents, like the admin user or any posts you have prepared. For that, you'll export your current local database and import it to the one you have on fortrabbit.

Please see the [MySQL article](mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer and the various ways to export / import the database. 


### Setup Git and deploy

While being into your **local** WordPress installation folder within the terminal, initialize your local Git and add your fortrabbit App as the remote like so:

```
# 1. Initialize Git
$ git init

# 2. Add the fortrabbit App remote, so you can git push later on
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 1. Add all changes to Git
$ git add -A

# 2. Commit changes
$ git commit -am "Initial"

# 3. Push to deploy (and set the upstream)
$ git push -u fortrabbit master
```

That first deployment will take a little longer as Composer will also run during the deployment. From there on you can simply only use `git push`.

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool, finally you can visit your App in the browser and follow the WordPress setup:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Final configuration

While you can already login and use WordPress, further essential steps need to be done:


### Permalinks

WordPress creates an `.htaccess` file during setup on the fly, also see our [.htaccess help](/htaccess). With [ephemeral storage](quirks#toc-ephemeral-storage) this will be destroyed during each deployment. We'll make the `.htaccess` permanent. To do so, open the hidden `.gitignore` file on top level and remove the .htaccess file from being ignored from Git by deleting or commenting the line like so:

```
# WordPress
web/wp
#web/.htaccess
```

Next, create an `.htaccess` file with following contents and also place it on top level.

```
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

Then in your local terminal, deploy the changes:

```
# 1. Add .htaccess & modified .gitignore to Git
$ git add -A

# 2. Commit changes
$ git commit -am "Added htaccess"

# 3. Push to deploy htaccess
$ git push
```

### Object storage

Since WordPress is a CMS living on editorial provided content you most likely need a persistent storage — but fortrabbit App have [ephemeral storage](/quirks#toc-ephemeral-storage). That's not a breaker: Use our [Object Storage](/object-storage). Once you have booked the Component in the Dashboard the credentials will automatically become available via the [App secrets](/secrets).

Now you install a plugin locally. Best do it with Composer in your local terminal:

```bash
$ composer require deliciousbrains/wp-amazon-s3-and-cloudfront
```

Next create a new file `web/app/plugins/amazon-s3-fortrabbit.php` with the following contents:

```
<?php
/*
Plugin Name: Amazon S3 fortrabbit
*/

$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
define('DBI_AWS_ACCESS_KEY_ID', $secrets['OBJECT_STORAGE']['KEY']);
define('DBI_AWS_SECRET_ACCESS_KEY', $secrets['OBJECT_STORAGE']['SECRET']);

add_filter('aws_get_client_args', function($args) use ($secrets) {
    $args['endpoint'] = 'https://'. $secrets['OBJECT_STORAGE']['SERVER'];
    return $args;
});
```

To make sure the plugin becomes part of the Git repo you need to open the `.gitignore` file and edit it like so:

```
# …
web/app/plugins/*
!web/app/plugins/.gitkeep
!web/app/plugins/amazon-s3-fortrabbit.php
# …
```

Now, in your local terminal commit everything and push it online:

```bash
$ git commit -am "With Object Storage"
$ git push
```

Once the deployment is done, you can head over to the WordPress Admin on the fortrabbit App, activate all three plugins ("Amazon S3 fortrabbit", "Amazon Web Services" and "WP Offload S3 (Lite)") and then navigate to AWS > "S3 and CloudFront". First choose your bucket (there will be only one with the same name as your App), then enable "CloudFront or Custom Domain" and enter `{{app-name}}.objects.frb.io` as a custom domain. Save and done! Now, uploads to your WordPress will go directly to the Object Storage.


## Tuning

### Installing plugins

[Bedrock allows](https://roots.io/bedrock/docs/composer/#plugins) two ways to install plugins:  Use Composer to install plugins from [WordPress Packagist](http://wpackagist.org/); Just download and unpack the plugins into `web/app/plugins`

#### With Composer

This is the recommend way, since it already provides a simple upgrade mechanism (Composer, of course). Following is an example on how to install Akismet:

```bash
$ composer require wpackagist-plugin/akismet
$ git commit -am "With Akismet"
$ git push
```

Now you should enable the Plugin via the WordPress admin. Not all plugins are, as of yet, available via Wordpress Packagist, but the number is growing. Use their [search](http://wpackagist.org/) to check if the plugin you require is available.

#### Without Composer

If the plugin is not (yet) available via WordPress Packagist, you can still use the "old way" to install plugins. Following an example to install the WooCommerce plugin:

```bash
$ cd {{app-name}}/web/app/plugins
$ wget http://downloads.wordpress.org/plugin/woocommerce.zip
$ unzip woocommerce.zip
$ rm woocommerce.zip
```

Now make sure that the WooCommerce plugin directory is excluded from the `.gitignore` (aka: un-ignored) by opening `{{app-name}}/.gitignore` and adding a new line immediately after `!web/app/plugins/.gitkeep`:

```
# other code …
web/app/plugins/*
!web/app/plugins/.gitkeep
!web/app/plugins/woocommerce
# other code …
```

Now you can add everything to Git and push it online:

```bash
$ cd {{app-name}}
$ git add -A
$ git commit -m "With WooCommerce"
$ git push
```

Once that's done you can head over your WordPress admin and activate the plugin as usual.

### Installing themes

Bedrock recommends to install themes by unpacking them into the `web/app/themes` folder and to use Composer only in [rare cases](https://roots.io/bedrock/docs/composer/#themes).


### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:
