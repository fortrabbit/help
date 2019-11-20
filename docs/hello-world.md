---

template:      article
reviewed:      2019-10-30
naviTitle:     Hello world
title:         Hello world tutorial with Git
lead:          This is the famous 3 minute tutorial to get started with fortrabbit. It uses plain simple PHP without any framework or CMS. See how fast and easy you can get your code up and running.
stack:         all


keywords:
    - tutorial
    - guide
    - git-deployment
    - Git
    - terminal
    - shell
    - bash

---

## Get ready

Let's have a quick check-up before we get started here: You have a [PHP development environment](/local-development) or at least [Git](/git) running on your local machine. You have already [signed up to fortrabbit](//dashboard.fortrabbit.com/signup), have created a plain vanilla PHP [App](app) - make sure to not select any framework or CMS (or if you did, change the root path back to htdocs in the Dashboard).


## Deploy a hello world example with Git

The following code gets executed on your local machine in your terminal application. The lines starting with a `$` are the ones you execute. The following code examples include the Git clone URL from your App: **{{app-name}}**.

```bash
# 1. Clone the repo — Initialize Git by cloning the empty repo from fortrabbit
$ git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
# Cloning into '{{app-name}}'...
# warning: You appear to have cloned an empty repository

# 2. Change directory to your new local project
$ cd {{app-name}}

# 3. Create an index php file with a bold messsage on the fly
$ echo '<?php echo "PHPower to the PHPeople";' > index.php

# 4. Add the change in the working directory to the staging area
$ git add index.php

# 5. Commit the staged snapshot to the project history
$ git commit -am 'initial commit'

# 6. Set upstream and 1st push
$ git push -u origin master
# Counting objects: 3, done.
# Writing objects: 100% (3/3), 251 bytes | 0 bytes/s, done.
# Total 3 (delta 0), reused 0 (delta 0)

# Commit received, starting build of branch master
#
# –––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
#
# B U I L D
#
#
# Checksum:
#   3ec951f4f4a59a42afa7e6bebaa78477672d3b6a
#
# Deployment file:
#   not found
#
# Pre-script:
#   not found
#   0ms
#
# Composer:
#   not executing (composer.json missing)
#   0ms
#
# Post-script:
#   not found
#   0ms
#
#
#
# R E L E A S E
#
#
# Packaging:
#   6ms
#
# Revision:
#   1446568383255266100.3ec951f4f4a59a42afa7e6bebaa78477672d3b6a
#
# Size:
#   182B
#
# Uploading:
#   134ms
#
# Build & release done in 217ms, now queued for final distribution.
#
# –––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
#
# To {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
#  * [new branch]      master -> master
# Branch master set up to track remote branch master from origin.

# 7. Every deploy from now on
$ git push
```
**Got an error?** Please see [access troubleshooting](/access-methods#toc-troubleshooting) and [Git troubleshooting](/git).
**Did it work?** Cool! That's it. You now visit your App URL in the browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

### Further readings

This was just a simple example. Don't miss our guides to: 

* [Install Laravel](install-laravel)
* [Install WordPress](install-wordpress)
* [Install Symfony](install-symfony)
* [Install Craft CMS](install-craft)
* [Install Drupal](install-drupal)
