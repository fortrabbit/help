---

template:      article
reviewed:      2024-02-15 08:20:08
title:         Advanced Git deployment workflows with fortrabbit.yml
naviTitle:     Deployment file
lead:          Enhance your deployment process with the fortrabbit.yml deployment file.
group:         deployment
stack:         all
order:         10

keywords:
   - advanced
   - git


---

## Problem

[Git push to deploy](git) is nice and easy to get your code up and running quickly. But deployment is not just deployment — there is a huge variety of use cases and preferred workflows. The `fortrabbit.yml` is an optional helper to support your habits. With the deployment file version you can add post- or pre-deploy scripts and control how Composer behaves.


## Solution

Create a file named `fortrabbit.yml` in the App's root folder of your project and include it into Git. After each push, the friendly fortrabbit bots will look for the file and parse it if exists.


## Full schema

```yml
# differentiate from the deployment files
version: 2

# called before Composer runs, so dependencies/libraries not guaranteed to be in place (must be a single PHP script)
pre: my-script.php arg1 arg2 arg3

# optional Composer settings
composer:

    # Per default dist is preferred
    prefer-source: false

    # Resolves to the --no-dev parameter, default is false
    no-dev: false

    # Resolves to the --no-plugins parameter, default is false
    no-plugins: false

    # Resolves to the --no-scripts parameter, default is false
    no-scripts: false

# called after Composer runs (must be a single PHP script)
post: my-script.php arg1 arg2 arg3

# list of sustained folders in ~/htdocs. If not given, then it defaults to the "vendor" folder
sustained:
    - vendor

```


## A minimal example

A fortrabbit.yml which calls the PHP script `post.php` on top-level of the repo after Composer looks like this:

```yml
version: 2
post: post.php

```

## Developing pre or post calls

When developing pre/post scripts, it helps if you are able to execute them once and see what they do. To that end you can use [remote SSH commands](/remote-ssh-execution-pro).

Bear in mind the pre script won't have access to dependencies/libraries during your first deployment, since `composer install` hasn't ran at that time. Subsequent deployments will have those files however, since the `/vendor` directory is sustained - unless you reset the repository.

Only a single pre/post script will be executed, chaining won't work. If you need to run multiple scripts, you'll need to create a wrapper script and require your other scripts within.


## Multi staging use case

To allow using a single git repo with [multiple remotes](multi-staging) you can use an App name based deployment file name.

Assuming you are using two Apps, `your-app-prod` and `your-app-stage`, you can set up two deployment files with names: `fortrabbit.your-app-prod.yml` and `fortrabbit.your-app-stage.yml`. This way, you can have both deployment files in a single repo but which one is to be used is determined by the App you deploy to.


## Private composer use case

The need for a pre script is rare, but if you [use private repositories](/private-composer-repos) over SSH (i.e. rather than `github-oauth`) and are concerned about a (highly unlikely) MITM attack, you can add a simple pre script as follows:

```php
<?php
// Prevent MITM, by enabling StrictHostKeyChecking and adding a verified host for github.com before composer SSH installs
// Changes are sustained, so could be done once manually (and after resetting the repo) but safer to automate

$fortrabbitSshConfigPath  = '/app/.ssh/config';
$fortrabbitKnownHostsPath = '/app/.ssh/known_hosts';

file_put_contents($fortrabbitKnownHostsPath, '1.2.3.4 ssh-rsa ABC...paste in here...DEF==' . PHP_EOL);
file_put_contents($fortrabbitSshConfigPath, str_replace(
    ['StrictHostKeyChecking no', 'UserKnownHostsFile /dev/null'],
    ['StrictHostKeyChecking yes', 'UserKnownHostsFile ' . $fortrabbitKnownHostsPath],
    file_get_contents($fortrabbitSshConfigPath)
));
```

## Troubleshooting

### Changes from scripts are not affecting the file system of the App

Mind the design of the fortrabbit infrastructure: 

* the deployment service is separated from the web delivery service
* the deployment file will be executed on the deploy service, not on the App
 
So calling a script that - for example - is cleaning up some runtime data created by the App itself will not work, since the runtime data is on the App and not on the deploy service. The result of the deployment file will be deployed.