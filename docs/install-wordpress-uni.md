---

template:         article
reviewed:         2023-05-11 20:13:38
title:            Install WordPress
naviTitle:        WordPress
group:            Install_guides
stack:            uni
lead:             Learn how to install and tune the popular blogging and CMS engine WordPress on fortrabbit.
deprecated:       0
stack:            uni

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          6.2
supportLevel:     b

keywords:
    - wp-admin

---


## Get ready

We assume you've already created an [App](app) and chose WordPress in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard). Following the fastest way to start with a fresh installation. Please scroll below for [migrating an existing WordPress](#toc-advanced-setup). 

This install guide was last tested with version you can see on the right but believed to work with older versions in the exact same way.

## Quick start

### 1. Download WordPress

Start by downloading the [latest WordPress archive](https://wordpress.org/latest.zip) from the WordPress website and unpack it locally. It will extract into the folder `wordpress`.

### 2. Upload WordPress files

Now copy the **contents** of the local `wordpress` folder (not the folder itself) via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

The upload will take around 7 minutes, depending on your internet speed. A quicker method is shown [below](#toc-installing-wordpress-with-ssh).

### 3. Run the installer

When the upload is finished, visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in your browser and commence with the guided web installation to finish the setup. You will need MySQL access, which is:

* **Database name**: `{{app-name}}`
* **User name**: `{{app-name}}`
* **Password**: [Look up in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql)
* **Database host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_
* **Table prefix**: keep `wp_` or your choice

The installer will also ask you for your WordPress site's **Site title**, **User name**, **Password**, **Your email** and so on.

After the guided web setup is done, you will be automatically redirected to the WordPress admin login form. Use the previously chosen Username and Password to login. Your WordPress site is now live under 😋:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< WordPress installation_
* [{{app-name}}.frb.io/wp-admin](https://{{app-name}}.frb.io/wp-admin) _< WordPress admin_

## Advanced setup

**Don't stop with a plain vanilla installation. Make it yours!** Check out the following topics if you have an existing WordPress installation or if you would like to set up WordPress so that you can run in a local development environment as well as in your fortrabbit App:

### Set up environment configuration

We recommend having your WordPress application running in at least two installations: [Locally](/local-development) and with your fortrabbit App here. Using [environment variables](env-vars) in your `wp-config.php` instead of hard coded credentials allows you to leverage [environment detection](local-development#toc-environment-detection): run your WordPress locally and remotely without code or configuration file changes.

Check if the file `wp-config.php` exists at the top level of your local WordPress installation. If it doesn't: create it by making a copy of `wp-config-sample.php` and naming it `wp-config.php`. Now open `wp-config.php` in an editor and modify it using [environment variables](/env-vars) as follows:

```php
/** The base configuration for WordPress */
/**                    grab the ENV var              or use local credentials  */

define('DB_NAME',      getenv('MYSQL_DATABASE')      ?: 'your-local-db-name');
define('DB_USER',      getenv('MYSQL_USER')          ?: 'your-local-db-user');
define('DB_PASSWORD',  getenv('MYSQL_PASSWORD')      ?: 'your-local-db-password');
define('DB_HOST',      getenv('MYSQL_HOST')          ?: 'localhost');
define('DB_CHARSET',  'utf8');
define('DB_COLLATE',  '');

/** Authentication Unique Keys and Salts. */
/**                        grab the ENV var           or  use local credentials  */

define('AUTH_KEY',         getenv('AUTH_KEY')         ?: 'unique local phrase');
define('SECURE_AUTH_KEY',  getenv('SECURE_AUTH_KEY')  ?: 'unique local phrase');
define('LOGGED_IN_KEY',    getenv('LOGGED_IN_KEY')    ?: 'unique local phrase');
define('NONCE_KEY',        getenv('NONCE_KEY')        ?: 'unique local phrase');
define('AUTH_SALT',        getenv('AUTH_SALT')        ?: 'unique local phrase');
define('SECURE_AUTH_SALT', getenv('SECURE_AUTH_SALT') ?: 'unique local phrase');
define('LOGGED_IN_SALT',   getenv('LOGGED_IN_SALT')   ?: 'unique local phrase');
define('NONCE_SALT',       getenv('NONCE_SALT')       ?: 'unique local phrase');

/** WordPress Database Table prefix. */
$table_prefix  = 'wp_';

/** For developers: WordPress debugging mode. */
define('WP_DEBUG', getenv('WP_DEBUG') ? true : false);

/* That's all, stop editing! Happy blogging. */

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

### Database migration

WordPress consists of code files, the user generated uploads and of course the [MySQL database](/mysql), in which most of the contents are stored. There are various use cases to export and import the database:

1. Export the database from your old web hosting
2. Export your local database to import it to the fortrabbit database
3. Export the remote database from fortrabbit to bring your local installation up-to-date

#### Migrating the database with a GUI

Read on in the [MySQL Article: Export & import > Using MySQL Workbench (GUI)](mysql-uni#toc-using-mysql-workbench-gui-).

#### Migrating the database in the terminal

Read on in the [MySQL Article: Export & import > Using the terminal](mysql-uni#toc-using-the-terminal).

### Developing WordPress

Continuous development of a WordPress site has different requirements than first time setup. Speed and certainty of deployed code are of utmost importance.

#### Uploading WordPress changes with SFTP

You can simply use your preferred [SFTP](/sftp) tool to upload your code modifications. This does not need further explanation: Just upload your changed files. The advantage is that it's easy as pie. The disadvantage is that it's slow and one can get confused about which changes are online and and which are not.

#### Syncing WordPress changes with SFTP

One strategy to simplify your life is to use SFTP synchronization. Please see the dedicated [section in the SFTP article](sftp#toc-file-synchronization) on that.

#### Deploying WordPress changes with rsync

Sophisticated developers might have a look at rsync. It's many times faster than SFTP. See our [rsync article](/rsync).

#### Deploying WordPress changes with Git

WordPress itself has not arrived in the new PHP age in terms of using the latest technologies and paradigms. So — we do not recommend using the standard WordPress with Git and Composer. But there is a super-cool WordPress boilerplate called [Bedrock](https://roots.io/bedrock/). It's required to run WordPress on the Professional Stack, but can also be used on the Universal Stack.


### Installing WordPress with SSH

This is an alternative, much quicker and more advanced way to install WordPress via shell commands. Issue the following in your local terminal:

```bash
# 1. Login to your App via SSH
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com

# 2. Download latest Wordpress
$ wget https://wordpress.org/latest.tar.gz

# 3. Unpack wordpress
$ tar zxf latest.tar.gz

# 4. Move unpackged files to top level
$ mv wordpress/* .

# 5. Remove the downloaded container
$ rm -r wordpress latest.tar.gz
```

Now proceed with the web installer as shown above. Combine this workflow with SFTP or SSH/rsync deployment methods. When you install WordPress directly on the App, the contents will not be represented in Git.

#### Using the WP-CLI

You can also use the popular and handy WordPress Command Line Interface on your fortrabbit App. When logged in to your fortrabbit App via SSH, you can just issue this command:

```
$ wp
```

The first time you call this will install it, next time you can just use it. See a [list of commands here](https://developer.wordpress.org/cli/commands/).


### Adding a custom domain

Your App URL `{{app-name}}.frb.io` is the first address your WordPress can be reached. Later on, when you go live, you will add your own custom external domains. Here is the basic setup:

1. Register and route the domain to the fortrabbit App < see [domain article](/domains)
2. [Connect the domain to your fortrabbit App](/domains#toc-connect-your-domain-to-fortrabbit), so that requests for a new domain will be delegated to your App
3. Once the domain is routed, tell WordPress to use the new domain as well:

#### Changing the site_url domain in the wp-admin

In the WordPress admin (https://{{app-name}}.frb.io/wp-admin) change the Site URL from your App URL to that new domain. Find this setting in wp-admin under Settings > General: "WordPress Address (URL)" and "Site Address (URL)". Change this to your new domain. More advanced help regarding domains and the vars `home_url` and `site_url` can be found in the Wordpress codex article on [changing the Site URL](https://wordpress.org/support/article/changing-the-site-url/).


### Installing themes & plugins

These are pretty standard operations. You can download and update themes directly from the WordPress admin. Or you can create your own, test them locally, then upload them to your remote themes folder. The same applies to plugins.


### Keeping WordPress secure

Urgent security advice: WordPress is popular with hackers. You are responsible to keep the software you install up-to-date — see our [security guidelines](https://www.fortrabbit.com/security). The good news is that WordPress has automatic background updates and they are enabled by default. Please check the article from the offical WordPress codex on how to [configure automatic background updates](https://wordpress.org/support/article/configuring-automatic-background-updates/) and take care that your WordPress core, plugins and even themes are always [up-to-date](#toc-updating-wordpress).


#### Protect against bots

WordPress often gets targeted by bots, trying to access `wp-login.php` and `xmlrpc.php`.  Blocking access to these files will increase stability and security.


### Sending e-mails

You cannot use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for the `wp_mail()` function.


### Resetting your password for wp-admin

Some users might be too lazy to configure the mail delivery for WordPress via SMTP (see above). Now imagine they also forget the password for `wp-admin`. Without email, the forgot password function from WordPress will not work. The user can still set a new password in the database. Like so: 

1. Connect to the remote MySQL database from local (see [here how](mysql#toc-access-mysql-database-from-local))
2. Browse the MySQL tables to find the right admin user
3. Choose a safe password
4. Convert that password to a MD5 hash
5. Update the table with the new password
6. Go back to wp-admin and login using the new password

### Running WordPress in a sub folder

There are two reasons to install WordPress in a sub directory instead of in `htdocs`:

1. WordPress is just the blog-part of the website: `mydomain.com/blog`
2. You want to run multiple WordPress sites in one App. [Please don't.](/app#toc-one-website-per-app)

You can achieve the first option by putting WordPress in a folder and by changing the "Site Address URL" parameter (see above). Also see the official WordPress codex on how to [give WordPress it's own directory](https://wordpress.org/support/article/giving-wordpress-its-own-directory/).

### Working with multisite

Usually we advise using one App per website (see [here](/app#toc-one-website-per-app)). But you can use the website "network" feature (previously multi-site) with one WordPress installation here as well. You might point different domains to one App.


## Updating WordPress

As you have seen above, there are many ways to deploy and develop WordPress. Depending on your work-flow, the way you'll update WordPress and deploy the update might be different. Minor patch versions of WordPress might be updated automatically.

With WordPress, except when using [Bedrock](https://roots.io/bedrock/) or the like, most likely you will just use the update functionality provided by the WordPress admin. Log in to `wp-admin`: when an update is available, it will inform you and display an "update" button you can click. That will trigger the update.

See also the [official WordPress docs on updating](https://wordpress.org/support/article/updating-wordpress/).

### Update locally first

We recommend to have a [local development environment](/local-development) and do the updates locally first and then deploy the changes to the App on fortrabbit. That way you can make sure that everything works in development before doing something in production. 

While updating the WordPress core, it's likely that the database design will change. The update script will do a migration, converting the tables to the new design while keeping their contents. When you run the installer in your local development and then deploy the new files only, the database version and the file versions do not match. Most likely you will see a warning in "wp-admin" to run the migration from there. 


### Parallel updates

To avoid migration struggles you can also keep your local and your remote version of WordPress in sync by running the updates in both environments, one by one. You might start with the local version and when successful, run the installer again on the App itself. This works with [Universal Apps](/app-uni), as the file storage is persistent. The disadvantage of this strategy is that your versions may slightly differ.

### Updating plugins and themes

So far we have covered updating the WordPress core, but WordPress wouldn't be WordPress without the eco-system of themes and plugins. Those can and have to be updated as well. The same mechanisms as above apply.


## Keeping your environments in sync

You know that WordPress is a CMS: **C**ontent can be **M**anaged in that **S**ystem. So in consequence that means that content is likely to be added and edited on the system — the App — itself. This content could be blog posts, pages and also media like images. Maybe you are adding them, maybe the client, maybe the editors. Now, if you have been following our recommendation to have a local development environment, you might find yourself asking: **How can I get the contents from the App back into the local development?**

A little refresher first: WordPress stores the text content of posts and pages in the MySQL database, uploads are stored on the file system. So you need to sync two things:

For the **database**, check our [MySQL article for import/export](/mysql). For the **files** you can either use [SFTP](/sftp) or have a look at [rsync](/rsync) to synchronize certain folders.

The techniques described work in both directions. Most likely you want to get the newest contents from the App into your local development environment, but maybe have you local changes that you would like to see on the App as well.