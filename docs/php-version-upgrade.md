---

template:      article
reviewed:      2022-10-12 10:46:36
title:         Upgrading the PHP version
naviTitle:     Upgrading the PHP version
lead:          This PHP upgrade path guides you through best practices upgrading the PHP version for your fortrabbit App.
group:         tips
stack:         all

keywords:
    - app-design
    - deprecation
    - migration
    - EOl
    - mcrypt

---

## Prerequisites

This is how PHP versions are handled with fortrabbit:

* fortrabbit will provide PHP updates once they come available
* New PHP versions are available in the fortrabbit Dashboard
* There are usually multiple PHP version to choose from available
* You need to update software you are using yourself
* You will need to change to a newer PHP version yourself
* Outdated PHP versions will be remove by fortrabbit (see [supported PHP versions](https://www.php.net/supported-versions.php))
* Apps on outdated PHP versions will be upgraded to the next PHP version available

Also see the [fortrabbit update policies](https://www.fortrabbit.com/update-policies).

## Simple PHP version upgrade path

This is the simple way to update your App in 5 minutes. It should work for 90% of the Apps here, especially when you have been keeping your software in use up-to-date (which you are required to do):

1. Make sure the software you are using is up-to-date
2. In the [fortrabbit Dashboard](https://dashboard.fortrabbit.com/)
3. Navigate to the App and then click on Settings > PHP
4. Change to a newer PHP version and hit "save"
5. Check if your website is still working after a few minutes
6. You can safely switch back the version if it doesn't work

This will update the PHP runtime on fortrabbit. You may also want to update your local development environment as well, read on.

## Sophisticated PHP version upgrade path

If the above procedure is breaking your website or you don't want to risk your production environment, here is a better, more advanced way. First update your local version, once that is working, deploy to fortrabbit and change the version. Here are the steps one by one:

### 1 - Update your local development environment

We recommend to have a local development environment, also see our [help article on that](/local-development). So the first thing you need to do is make sure that your local PHP version is up-to-date. Depending on how your local development is set up, the path to upgrade is different.

If you are running PHP directly on your computer, then a simple `php -v` prints out the PHP version. Beware, if you are using [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html), this is not the version of PHP your code runs on. With these tools you have to use their GUI to select the PHP version you want.

To upgrade your local PHP on macOS, [Homebrew](https://brew.sh/) is a popular package manager you can use. Homebrew also controls the PHP version used by [Laravel Valet](https://github.com/laravel/valet). But if you are using [Valet+](https://github.com/weprovide/valet-plus) that tool has built in support for switching PHP versions (fancy!).

**Beware:** If you want to use a one-click-update feature of a CMS like WordPress, CraftCMS or similar. You have to run that one-click-update locally BEFORE upgrading your local PHP, otherwise the admin GUI (wp-admin or control panel) might break.

When you are running a more complex setup with PHP virtualized, such as Craft Nitro, Vagrant, Virtual Box or Docker, you have to dig in to your specific virtualization setup to see how to upgrade PHP.

### 2 - Update your software dependencies

Get your dependencies up-to-date:

#### 2.1 - Updating with Composer

Applications based on PHP frameworks like Laravel and Symfony are usually updated with Composer which keeps track of all dependencies.

Issuing `composer outdated` in the Terminal will give you a list of outdated packages. Those in red need can easily be updated. Those in yellow also need to be updated but might cause trouble because they are major version upgrades.

You can also ask Composer why you currently can not upgrade to a PHP version:

```php
composer why-not php 8
```

To update a dependency, simply change any required versions in your `composer.json` to a newer version and then issue a `composer update` to actually install the required updates.

Keep in mind that the  `composer outdated` command does not care about PHP versions. It will only tell you about available updates for your dependencies, but hopefully newer packages should support newer PHP versions as well.

#### 2.2 - Updating a CMS

Many Content Management Systems, like WordPress, Craft CMS and Grav come with a built in update feature. So you can simply login to the admin area of the CMS and hit a button to update.

**Beware:** With fortrabbit, those changes only happen on the file system and are not reflected in Git. This is fine when you are using SFTP, otherwise read more in our [setup guide for WordPress](https://help.fortrabbit.com/install-wordpress-4-uni#toc-updating-wordpress) or [update guide for Craft CMS](https://help.fortrabbit.com/craft-tune#toc-updating-craft).

**Tip:** WordPress also has a handy plugin to check whether your other plugins will work with the latest PHP version: [PHP Compatibility Checker](https://wordpress.org/plugins/php-compatibility-checker/).

### 3 - Test it locally

When you have upgraded your local PHP version as well as your projects dependencies, it's time to make sure nothing is broken! Open your browser and try out as many views and actions as you can in your local development setup.

If you are running a lot of custom code you have written yourself, we recommend that you run an automated code analysis. It checks your code and finds any deprecated functions that might break.

### 4 - Go live with the updated version

Now, as everything is up-to-date and tested in your **local** development environment, it's time to bring the updates to fortrabbit.

#### 4.1 - Change the PHP version on fortrabbit

Updating the PHP version for your fortrabbit App is as simple as pie:

1. Visit your App in the fortrabbit Dashboard
2. Go to the PHP settings
3. Select a newer PHP version from the drop-down menu
4. Hit save

Changes can take two minutes to be applied. You can also test-run this. When your App is not running under the newer version of PHP you can switch back to the deprecated version for another while and find out why first.

#### 4.2 - Deploy changes

Now, quickly after the new PHP version is in place, deploy your updated code from your local development to your fortrabbit App, either with [Git](/git-deployment) or simply by [SFTP](/sftp-uni) or by [rsync](/rsync).

Don't forget that you might also have to run database migrations so that your database structure matches the latest code version. WordPress and Craft CMS automatically handle this in the web UI. For any other system, see their documentation on how to properly update the database.

That's it.
