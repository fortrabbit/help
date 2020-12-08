---

template:      article
reviewed:      2020-12-10
naviTitle:     Composer 2
title:         Git deployment issues with Composer 2
lead:          How to solve deployment issues related to Composer 2
group:         deployment
stack:         tips

workInProgress: yes
dontList: true


keywords:
    - composer
    - git
    - craft
    - laravel

---

## Composer 2

At 2020-12-XX we switched to Composer 2.x with our git deployment. It makes the deployments with composer much faster, but in some edge cases deployments might break. Composer 1.x is not actively maintained, so we recommend to upgrade to Composer 2 sooner or later.


## Fix Laravel 5.5, 5.6, 5.7, 5.8, 6, and 7 

Git deployments with older versions of Laravel might stop working. Luckily a patch [#32309](https://github.com/laravel/framework/issues/32309 was applied to all previous versions. Make sure to update Laravel locally:

```
# Update composer locally
composer self-update --2

# Update laravel locally
composer update laravel/framework
```


## Fix Craft 3


Git deployments with older versions of Craft might stop working throwing errors like this:

```
Failed to extract vendor/plugin-name
```

Solution: Update Craft to 3.5.15 or higher


## How to use Composer 1?


There might be another other reason why Composer 2 does not work for your, like packages that don't follow PSR-4 naming conventions.
In this case you may don't want to use Composer 2. With a little effort you can stick with Composer 1: 

Create a `fortrabbit.yml` that disables the default composer behaviour and calls a custom script instead:

```
pre: custom-composer.php
composer:
    disabled: true
```


The custom script (custom-composer.php):

```
<?php

// Download installer
copy('https://getcomposer.org/installer', '/tmp/composer-setup.php');

// Install a specific major version
echo shell_exec('php /tmp/composer-setup.php --install-dir=/tmp --1');

// Execute custom composer
echo shell_exec('php /tmp/composer.phar install --prefer-dist -n -d $PWD');
```

Put both files in the root of your project, commit the changes and use it as long as Composer support the 1.x branch.

