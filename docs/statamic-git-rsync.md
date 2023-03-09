---

template:         article
reviewed:         2023-03-07
order:            2
title:            Deploy Statamic with Git and rsync
naviTitle:        Statamic Git + rsync
lead:             Learn here our recommended way to deploy Statamic with Git and rsync on fortrabbit.
group:            statamic
stack:            uni
dontList:         false

websiteLink:      https://statamic.com/
websiteLinkText:  statamic.com
category:         CMS
image:            statamic-mark-new.svg
version:          3
supportLevel:     b

keywords:
    - statamic
    - starterkit
    - markdown
    - laravel
    - flatfile
    - headless

---


## Get ready

You should have Statamic running on your local machine, have a good understanding of Git and rsync, have an App on fortrabbit ready. Read our [Statamic intro](/statamic) first.

### Git and rsync

You'll deploy to fortrabbit using Git (and Composer) and (optionally) synchronize contents with rsync. 

#### 2.a - Configure Statamic for Git with rsync

Open your local Statamic project folder with your text editor or IDE. Within it open the `.gitignore` file at the root level to tell Git to ignore all content. We need to add any folders to `.gitignore` which contain user-generated content:

```gitignore
…
# Exclude stuff you are creating from Git
/content
/users
/resources/blueprints
/resources/fieldsets
/resources/forms
/resources/users
/storage/forms
/public/assets
```

PLEASE NOTE: This is our recommended way of doing it. It separates code from content. You will need to run dedicated rsync commands to deploy and update this user-generated content (see below). You can also decide to not touch the `.gitignore` file so that you can deploy everything with Git all together. Keep in mind that you can not pull in new contents from the fortrabbit App this way (see on work-flows above).

At that point you should be able to run the project in your local development environment already. We highly recommend developing the site locally, using fortrabbit for staging and production.

#### 2.b - Deploy Statamic with Git

In case you haven't already, set up Git, configure the fortrabbit App Git repo as a remote and push the code. Run this on your local computer:

```shell
# 1. Initialize Git
$ git init

# 2. Add your Apps Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'My first commit'

# 6. Initial push and upstream
$ git push -u fortrabbit main

# From there on only
$ git push
```

Also see our [Git deployment article](/git-deployment) for more details on Git here on fortrabbit.

#### 2.c - Synchronize content with rsync

As mentioned above, deployment of your code base (templates and configuration) and dependencies (Statamic and Composer) is done via Git deployment. Deploying the content is a separate step. We recommend using rsync to up- or down-load new contents to and from your remote fortrabbit App (see also our [rsync article](/rsync)). On your local computer in the terminal in the Statamic project folder execute:

```shell
# SYNC UP: from local to remote
$ rsync -avR ./content ./users ./resources/blueprints ./resources/fieldsets ./resources/forms ./resources/users ./storage/forms ./public/assets {{app-name}}@deploy.{{region}}.frbit.com:~/
```

(You may not have all these folders on your local system: Only include folders which you do have.)

It works also the other way around. For example, if you have some edits done online and want them to be reflected in your local development environment:

```shell
# SYNC DOWN: from remote to local
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/content ./
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/users ./
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/resources/blueprints ./
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/resources/fieldsets ./
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/resources/forms ./
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/resources/users ./
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/storage/forms ./ 
$ rsync -av '{{app-name}}@deploy.{{region}}.frbit.com:~/public/assets' ./
```