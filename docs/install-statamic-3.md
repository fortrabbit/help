---

template:         article
reviewed:         2020-02-08
title:            Install Statamic 3 on fortrabbit
naviTitle:        Statamic
lead:             Statamic is a cool file based CMS. Learn here how to install and tune Statamic 3 on fortrabbit.
group:            Install_guides
stack:            uni
dontList:         false
workInProgress:   true

websiteLink:      https://statamic.com/
websiteLinkText:  statamic.com
category:         CMS
image:            statamic-mark-new.svg
version:          3 beta
supportLevel:     c

keywords:
    - statamic
    - starterkit
    - markdown
    - laravel
    - flatfile
    - headless

---



## Document status

Mind that this install guide is work in progress and for the BETA version of Statamic 3. Our current knowledge on the inner workings of Statamic is limited. We have not much first hand experience. Make sure to use the official Statamic docs at [statamic.dev](https://statamic.dev/) as your main source of truth.

 
## Get ready

Make sure you to have completed all steps in the [get ready guide](/get-ready). Have a local development environment with PHP and a web server ready. Also best, already have a plain vanilla App at fortrabbit ready. There is no Statamic preset yet, so best choose "plain PHP" when bookng a new App in the fortrabbit Dashboard. 


## Install Statamic locally

Before you deploy anything to fortrabbit, we highly recommend to have your Statamic project running locally. This is how to start a project from scratch on your local machine (with a folder called like your App):

```
# 1. Create a local (on your own computer) Statamic project folder (called like your App) with Composer:
composer create-project statamic/statamic {{app-name}} --prefer-dist --stability=dev
```

From there on, some development might happen or you already have a project running locally, now:


## Setup

Next up, let's get Statamic ready that the same installation can run locally and on the fortrabbit App.

### Set environment variables

Statamic comes with a predefined (hidden) `.env` file. t includes what you'll need to run Statamic locally. Mind that the `.env` is ignored from Git. 

Best change the environment variables for your fortrabbit in our Dashboard. Go to your App in the Dashboard, under Settings find "ENV vars". You will be presented with a textarea to put in your "Custom ENV vars":

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

Statamic 3 is a Laravel application. So the root path needs to be set to `public`. Within the settings of the App under the Dashboard, open the form to change the root path, enter `public` and save the new value.

* [dashboard.fortrabbit.com/apps/{{app-name}}/rootpath](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath) <- direct link



## Choose your deployment work-flow

There are two "main" ways to deploy code here on fortrabbit: [Git](/git-deployment) and [SFTP](/sftp-uni). The general rule of thumb is, that content driven legacy applications, like [WordPress](/install-wordpress), are better uploaded in classical manner with SFTP. Modern PHP web frameworks that are based on [Composer](/composer) are mostly deployed with Git. 

Now, Statamic is a bit in between and is - like [Grav](/install-grav) and [Kirby](/install-kirby-3) - file based. So there is usually no database by default, the actual contents are text files written on the file system.

[Our architecture graphic here](/deployment-methods-uni#toc-understanding-the-architecture) shows you that the files, you can access via SFTP or SSH (Universal App) are not the ones, that are in Git. The Git repo is a separated thing. So, deploying with Git is a one way street that only goes up, not down (also [see here](/deployment-methods-uni#toc-git-works-only-one-way)). In other words: You can NOT `git pull` any changes you have made on the Apps file system. In an ideal world, code and content are maybe separated. With a file based CMS this is all together.


## SFTP upload

**Workflow 1 of 2** — There is not much to say on that topic. Just make sure to upload all contents of your local Statamic folder, including the hidden `.htaccess` file into the `htdocs` folder within your App.


## Deploy with Git and rsync

**Workflow 2 of 2 and recommended** — You'll  deploy to fortrabbit using Git ( and Composer ) and (optionally) synchronize contents with rsync. Let's go:


### Configure Statamic for Git deployment

Open your local Statamic project folder with your text editor or IDE. Within there open the (hidden) `.gitignore` file on top level to tell Git to ignore some folders. Add this to `.gitignore`:

```
…
# Exclude stuff you are creating from Git
/content
```

PLEASE NOTE: The setting above will also keep the `/content` folder out of Git. This is our opinionated way of doing it. It separates code from content. You will need to run dedicated rsync commands to deploy and update the "contents" (see below). You can also decide to not touch the `.gitignore` so that you can deploy everything with Git all together. Keep in mind that you can not pull in new contents from the fortrabbit App this way (see on work-flows above).

At that point you should be able to run the project in your local development environment already. We highly recommend to develop the site locally, use fortrabbit for staging and production.


### Deploy Statamic with Git

In case you haven't already, setup Git, configure the fortrabbit App Git repo as a remote and push code:

```
# 1. Initialize Git
$ git init .

# 2. Add your Apps Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'My first commit'

# 6. Initial push and upstream
$ git push -u fortrabbit master

# From there on only
$ git push
```

Also see our [Git deployment article](/git-deployment) for more details on Git here on fortrabbit.


### Synchronize content with rsync

As mentioned above, deployment of your code base (templates and configuration) and dependencies (Statamic and Composer) is done via Git deployment. Deploying the content is a separated step. We recommend to use rsync to up- or down-load new contents to and from your remote fortrabbit App (see also our [rsync article](/rsync)). On your local computer in the Terminal in the Statamic project folder execute:

```
# SYNC UP: from local to remote
$ rsync -av ./content {{app-name}}@deploy.{{region}}.frbit.com:~/
```

It works also the other way around. For example in a case, where you have done some edits online and want those changes to be reflected in your local development environment:

```
# SYNC DOWN: from remote to local
$ rsync -av Statamic-test@deploy.eu2.frbit.com:~/content ./
```


### Synchronize content with SFTP

You can also use [SFTP](/sftp) to synchronize the `content` folder. That's straight forward straight from the 90s: Open your SFTP client and drag the folder over, whatever direction you prefer.


## Tuning

By now, you have Statamic installation running on your local machine and you can easily deploy it to your fortrabbit App. You can deploy code changes and Statamic updated with Git. Additionally contents are synced down and up using rsync or Git. Let's get deeper:


### Adding domains

At the beginning you most likely have been using the App URL as your domain. You can also add any number of domains to your App. Please see our [domain article](/domains) for more.


### Using MySQL as a content store

_MYSQL is still WORK IN PROGRESS!_

Beside storing contents on the file system in markdown, Statamic also offers to store contents with a MySQL database. On fortrabbit, that might ( depends on your use case ) be a good option, since each Universal App comes with a MySQL database anyways and you don't have to go the extra round with rsync while still having good separation of code and content.

#### ENV vars for Statamic MySQL setup

MySQL access will be configured via ENV vars as well (see above on the matter as well). Copy/paste this additional setup into the textarea with environment variables settings form in the fortrabbit Dashboard:

```
DB_CONNECTION=mysql
DB_DATABASE=${MYSQL_DATABASE}
DB_HOST=${MYSQL_HOST}
DB_PASSWORD=${MYSQL_PASSWORD}
DB_PORT=3306
DB_USERNAME=${MYSQL_USER}
```

It maps the keys Statamic is expecting with dynamic ENV vars provided by fortrabbit.

_MYSQL is still WORK IN PROGRESS!_

### Working with the Control Panel

Statamic - like other CMS - has a "Control Panel". That Dashboard enables you - or maybe the client/editor - to create and edit the contents easily in the browser. You can create different users (admins) for the Control Panel.

* [{{app-name}}.frb.io/cp](https://{{app-name}}.frb.io/cp)

For this, please consider how you are dealing with the contents created: 

* You should really not use the Control Panel on the App when you have all contents in Git, in fact you should only change things locally. 
* When using rsync (or SFTP) you can sync changes you have made on the App directly back down or the other way around.
* When using MySQL as a data store, you might want to dump the production database and bring it back locally from time to time


### Continuos development

We recommend to always develop locally first — it's just the most convenient way. Deploy, when you have reached a certain status of development. In many cases the "real" content will be created on the fortrabbit App. You can easily sync down changes from production into development with rsync or sftp or you have the contents in MySQL.


### Backups

Some fortrabbit App plans have [backups enabled](/backups-uni). You can also spin your own solution. With Git you can already easily rollback changes. 


### Updating Statamic

We recommend to update your local development environment first. On your local computer issue `composer update` in Terminal on root level of the project folder to trigger the update. When you have confirmed that everything works, `git push` to bring the latest updates to your fortrabbit App.


### Sending mails

fortrabbit does not support `sendmail`, so sending mails out of the box might not work. Statamic provides config for transactional mail providers, or you can use a plugin to send e-mail over your own SMTP account.


### Getting a license

Remember that Statamic is not a free software. Get a license, support the authors!