---

template:         article
reviewed:         2023-01-05
title:            Install Statamic 3 on fortrabbit
naviTitle:        Statamic
lead:             Statamic is a cool file based CMS. Learn here how to install and tune Statamic 3 on fortrabbit.
group:            Install_guides
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

Make sure you to have completed all steps in the [get ready guide](/get-ready). Have a local development environment with PHP and a web server ready. Also have an App at fortrabbit ready. There is no Statamic preset yet, so choose "Laravel" (since Statamic is based on Laravel) when booking a new App in the fortrabbit Dashboard.

## Install Statamic locally

Before you deploy anything to fortrabbit, get your Statamic project running on your local computer. Please see the [official Statamic install guide](https://statamic.dev/installing/local) for up-to-date instructions. Also see our [local development article](/local-development) for options setting up a local development environment.

It would be good to already have your first admin user set up.

## Setup

Next up, let's get Statamic ready, so that the same installation can run locally and on the fortrabbit App.

### Set environment variables

Statamic comes with a predefined `.env` file. It includes what you'll need to run Statamic locally. Bear in mind that the `.env` is ignored in Git.

You should change the environment variables for your fortrabbit App in our Dashboard. Go to your App in the Dashboard, and under Settings find "ENV vars". You will be presented with a textarea to put in your "Custom ENV vars":

```env
APP_KEY=base64:nh4IjpE8duK2GDRG2hYouQjOcmFR9wtCNgKXDkAoMRw=
```

Have the same APP_KEY locally and on the App (copy from your local installation).

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://{{app-name}}.frb.io
```

The three ENV vars above will differ from your local installation. You can also add your own, of course. Please also see our [environment variables article](/env-vars) to learn more about that kind of configuration and our implementation.

### Set the root path

Statamic 3 is a Laravel application. So the root path needs to be set to `public`. This should be set already if you have chosen the Laravel software preset. 

* [dashboard.fortrabbit.com/apps/{{app-name}}/rootpath](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath) <- direct link

## Deployment workflow introduction

There are two "main" ways to deploy code here on fortrabbit: [Git](/git-deployment) and [SFTP](/sftp-uni). The general rule of thumb is, that content driven legacy applications, like [WordPress](/install-wordpress), are better uploaded in classical manner with SFTP. Modern PHP web frameworks that are based on [Composer](/composer) are mostly deployed with Git.

Now, Statamic is a bit in between and is - like [Grav](/install-grav) and [Kirby](/install-kirby-3) - file based. So there is usually no database by default, the actual contents are text files written on the file system.

[Our architecture graphic here](/deployment-methods-uni#toc-understanding-the-architecture) shows you that the files you can access via SFTP or SSH (Universal App) are not the ones that are in Git. The Git repo is a separate thing. This means that deploying with Git is a one way street that only goes up, not down (also [see here](/deployment-methods-uni#toc-git-works-only-one-way)). In other words: You can NOT `git pull` any changes you have made on the App's file system. In an ideal world, code and content are maybe separated. With a file based CMS this is all together.

## Deployment workflows documented

### 1 - SFTP upload

**Statamic deployment option 1** — There is not much to say on that topic. Just make sure to upload all contents of your local Statamic folder, including the hidden `.htaccess` file into the `htdocs` folder within your App.

### 2 - Git and rsync

**Statamic deployment option 2 (recommended)** — You'll deploy to fortrabbit using Git (and Composer) and (optionally) synchronize contents with rsync. Let's go:

#### 2.a - Configure Statamic for Git with rsync

Open your local Statamic project folder with your text editor or IDE. Within it open the `.gitignore` file at the root level to tell Git to ignore all content. Add this to `.gitignore`:

TODO: Add other folders!

```gitignore
…
# Exclude stuff you are creating from Git
/content
```

PLEASE NOTE: The setting above will keep the `/content` folder out of Git. This is our recommended way of doing it. It separates code from content. You will need to run dedicated rsync commands to deploy and update the "contents" (see below). You can also decide to not touch the `.gitignore` file so that you can deploy everything with Git all together. Keep in mind that you can not pull in new contents from the fortrabbit App this way (see on work-flows above).

At that point you should be able to run the project in your local development environment already. We highly recommend developing the site locally, using fortrabbit for staging and production.

#### 2.b - Deploy Statamic with Git

In case you haven't already, set up Git, configure the fortrabbit App Git repo as a remote and push the code. Run this on your local computer:

```shell
# 1. Initialize Git
$ git init .

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
$ rsync -av ./content {{app-name}}@deploy.{{region}}.frbit.com:~/
```

It works also the other way around. For example, if you have some edits done online and want them to be reflected in your local development environment:

```shell
# SYNC DOWN: from remote to local
$ rsync -av Statamic-test@deploy.eu2.frbit.com:~/content ./
```

### 3 - Sophisticated Git workflow

This is an advanced, opinionated and experimental workflow for experienced developers to deploy Statamic, where all content is stored as files and managed with Git using the Statamic Git Automation. See the [Git Automation Statamic docs article](https://statamic.dev/git-automation) to get the idea and as reference.

The idea is to create another repo on the App, which has the same upstream as your local repo: both the local and the App repos are sharing the same upstream. So the first step is to create this repo on the App, then create a new branch there, which is where we put any new content created on the App. (We create this new branch because we do not want to push changes to the master branch on the deploy service, because this would trigger a deployment.) Then, any changes we make to content on the App can then be pulled into your local repo, where it can be merged.

#### 3.a - Local preparation

1. Install Statamic on your local computer as described above.
2. Then follow the Git guide above, but leave out the `.gitignore` part under 2.a.

After you have finished with the first `git push`, your App should be online and you can login as admin with your user credentials on the App in the same way as you can do on your local computer.

#### 3.b - Get Statamic Pro

The Git Automation is part of the commercial Statamic version. You need to enable the Pro version and obtain a Statamic license to use that feature on fortrabbit. See the [official guide](https://statamic.dev/licensing) on how to do that.

As usual, do this with your local installation first and then push the latest state to fortrabbit. After this, you should be able to find Git with the utilities under the Statamic control panel.

#### 3.c - Set ENV vars in the fortrabbit Dashboard

Add the environment variables listed below to the App with the fortrabbit Dashboard.

```.env
STATAMIC_LICENSE_KEY=your-site-license-key
STATAMIC_GIT_ENABLED=true
STATAMIC_GIT_AUTOMATIC=true
```

#### 3.d - Prepare Git on the fortrabbit App

Login to your fortrabbit App by SSH and:

1. Set up an SSH key with SSH keygen (no passphrase), see our [SSH keys setup guide](/ssh-keys#toc-generate-a-ssh-key-pair-aka-ssh-key-)
2. Copy the content of the public key (likely `/srv/app/{{app-name}}/htdocs/.ssh/id_ed25519_fortrabbit.pub`) to a buffer
3. In the fortrabbit Dashboard create a new 'app-only SSH key' with the App, paste the contents of the public key file (open the file with Vim)
4. While logged in by SSH create a config file in the already existing folder `.ssh` by running `touch config`
5. Add the content below to the newly created config file (edit the file with Vim)

```config
# Add this to .ssh/config
# Use the App specific region, app name and SSH key name
Host deploy.{{region}}.frbit.com
    IdentityFile /srv/app/{{app-name}}/htdocs/.ssh/id_ed25519_fortrabbit
```

#### 3.e Init Git on the fortrabbit App

We need to create a separate branch to avoid triggering a deployment when pushing to the deploy service. So, make sure you are still logged in by SSH on the fortrabbit App.

First add a `.gitignore` file. Its contents should be the same as the `.gitignore` file on your local project. Then:

1. Initialise a new git repo: `git init`.
2. `git add .` & `git commit -m 'initial commit'`
3. Create an `editorial` branch and check it out (`git checkout -b 'editorial'`)
4. Add the fortrabbit deploy service as a remote (`git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git`)
5. Create a Git user (see example below)

```shell
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

See the [Statamic Git guide](https://statamic.dev/git-automation#remote-setup) and the [fortrabbit git remote help](/git-deployment#toc-adding-fortrabbit-as-a-remote) as reference.


#### 3.g Try it out

Still logged in by SSH on the fortrabbit App, you should be now able to add and commit files with Git to the App and push changes to the deploy service remote from the `editorial` branch.

When pushing for the first time, set the remote as an upstream: `git push -u fortrabbit editorial`.

To get this new branch locally, run `git fetch fortrabbit`.

With the Git repo on your local computer you can fetch and checkout the `editorial` remote branch. Pull changes from there and merge them back into your `main` branch that you are using for development.

#### 3.f - Git in the Statamic Control Panel

Now you can also use the Statamic Git Automation, depending on the setup, pending editorial changes are visible with the online Git editor in the Statamic control panel or get committed and pushed automatically (we advise using the recommended delay).


## Tuning Statamic

By now, you have a Statamic installation running on your local machine and you can easily deploy it to your fortrabbit App. You can deploy code changes with Git. Additionally contents are synced up and down using rsync or Git. Let's go deeper:

### Adding domains

At the beginning you have probably been using the App URL as your domain. You can also add any number of domains to your App. Please see our [domain article](/domains) for more.

#### ENV vars for Statamic MySQL setup

MySQL access will be configured via ENV vars as well. Copy/paste this additional setup into the environment variables form in the fortrabbit Dashboard:

```
DB_CONNECTION=mysql
DB_DATABASE=${MYSQL_DATABASE}
DB_HOST=${MYSQL_HOST}
DB_PASSWORD=${MYSQL_PASSWORD}
DB_PORT=3306
DB_USERNAME=${MYSQL_USER}
```

It maps the keys Statamic is expecting to dynamic ENV vars provided by fortrabbit.

### Working with the Control Panel

Statamic - like other CMS's - has a control panel. That dashboard enables you - or maybe the client/editor - to create and edit the contents easily in the browser. You can create different users (admins) for the control panel.

* [{{app-name}}.frb.io/cp](https://{{app-name}}.frb.io/cp)

For this, please consider how you are dealing with the content created: 

* You should really not use the control panel on the App if you have all content stored in Git: you should only change things locally. 
* When using rsync (or SFTP) you can sync changes you have made on the App directly back down or the other way around.
* When using MySQL as a data store, you might want to dump the production database and bring it back locally from time to time

### Continuous development

We recommend to always develop locally first — it's just the most convenient way. Deploy when you have reached a certain state of development. In many cases the "real" content will be created on the fortrabbit App. You can easily sync down changes from production into development with rsync or sftp unless you have the content in MySQL.

### Backups

Some fortrabbit App plans have [backups enabled](/backups-uni). You can also spin your own solution. With Git you can already easily roll back changes. 

### Updating Statamic

We recommend updating your local development environment first. On your local computer execute `composer update` in the terminal at the root level of your project folder to trigger the update. When you have confirmed that everything works, `git push` to bring the latest updates to your fortrabbit App.

### Sending mails

fortrabbit does not support `sendmail`, so sending e-mails out of the box might not work. Statamic provides config for transactional mail providers, or you can use a plugin to send e-mail over your own SMTP account.

### Getting a license

Remember that Statamic is free for personal use. For a client project, get a license, support the authors!
