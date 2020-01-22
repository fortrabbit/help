---

template:         article
reviewed:         2020-01-22
title:            Install Statamatic 3 on fortrabbit
naviTitle:        Statamatic
lead:             Statamatic is a popular, file based CMS. Learn here how to install and tune Statamatic on fortrabbit.
group:            Install_guides
stack:            uni
dontList:         false
workInProgress:   true

websiteLink:      https://statamic.com/
websiteLinkText:  statamic.com
category:         CMS
image:            statamatic-mark-new.svg
version:          3 beta
supportLevel:     c

keywords:
    - statamatic
    - starterkit
    - markdown
    - laravel
    - flatfile
    - headless

---



## Document status

Mind that this install guide is work in progress and for the beta version of Statamatic 3. Our current knowledge on the inner workings of Statamatic is also limited, we have not much first hand experience. Make sure to use the official Statamatic docs at [statamic.dev](https://statamic.dev/) as your main source.

 
## Get ready

Make sure you to have completed all steps in the [get ready guide](/get-ready). Have a local development environment with PHP and a web server ready.


## Install Statamatic locally

Before you deploy anything to fortrabbit, we highly recommend to have your Statamatic project running locally. This is how to start a project from scratch on your local machine:

```
# 1. Create a local (on your own computer) Statamatic project folder (called like your App) with Composer:
composer create-project statamic/statamic {{app-name}} --prefer-dist --stability=dev
```

From there on, some development might happen or you already have a project running locally, now:



## Choose your work-flow

There are two "main" ways to deploy code here on fortrabbit: [Git](/git-deployment) and [SFTP](/sftp-uni). The general rule of thumb is, that content driven legacy applications, like [WordPress](/install-wordpress), are better uploaded in classical manner with SFTP. Modern PHP web frameworks that are based on [Composer](/composer) are mostly deployed with Git. 

Now, Statamatic is a bit in between and is - like [Grav](/install-grav) and [Kirby](/install-kirby-3) - file based. So there is usually no database, the actual contents are text files written on the file system.

[Our architecture graphic here](/deployment-methods-uni#toc-understanding-the-architecture) shows you that the files, you can access via SFTP or SSH (Universal App) are not the ones, that are in Git. The Git repo is a separated thing. So, deploying with Git is a one way street that only goes up, not down. In other words: You can NOT `git pull` any changes you have made on the Apps file system. In an ideal world, code and content are maybe separated. With a file based CMS this is all together.


## SFTP upload

**Workflow 1 of 2** — There is not much to say on that topic. Just make sure to upload all contents of your local Statamatic folder, including the hidden `.htaccess` file into the `htdocs` folder within your App.


## Deploy with Git and rsync

**Workflow 2 of 2 and recommended** — Here is a basic rundown: 

1. Deploy to fortrabbit using Git ( and Composer )
2. Synchronize contents with rsync



### Configure Statamatic for Git deployment

Open your local Statamatic project folder with your text editor or IDE. Within there open the (hidden) `.gitignore` file on top level to tell Git to ignore some folders. Add those two lines:

```
…
# Statamatic itself
/Statamatic

# Composer dependencies
/vendor

# Stuff you are creating
/content
…
```

PLEASE NOTE: The setting above will also keep the `/content` folder out of Git. This is our opinionated way to do it. It will help keeping your repo tidy and separating code from content. But you will need to run dedicated rsync commands to deploy and update the "contents" (see below). You can also decide to exclude the `/content` with the `.gitignore` so that you can deploy everything with Git all together. Keep in mind that you can not pull in new contents from the fortrabbit App this way.

At that point you should be able to run the project in your local development environment already. We highly recommend to develop the site locally, use fortrabbit for staging and production.


### Deploy Statamatic with Git

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

### Synchronize your contents

As mentioned above, deployment of your code base (templates and configuration) and dependencies (Statamatic and Composer) is done via Git deployment. Deploying the content is a separated step. We recommend to use rsync to up- or down-load new contents to and from your remote fortrabbit App (see also our [rsync article](/rsync)). On your local computer in the Terminal in the Statamatic project folder execute:

```
# SYNC UP: from local to remote
$ rsync -av ./content {{app-name}}@deploy.{{region}}.frbit.com:~/content
```

It works also the other way around. For example in a case, where you have done some edits online and want those changes to be reflected in your local development environment:

```
# SYNC DOWN: from remote to local
$ rsync -av {{app-name}}@deploy.{{region}}.frbit.com:~/content ./content
```

You can also use [SFTP](/sftp) to synchronize the `content` folder.


## Tuning

By now, you have Statamatic installation running on your local machine and you can easily deploy it to your fortrabbit App. You can deploy code changes and Statamatic updated with Git. Additionally contents are synced down and up using rsync or Git. Let's get deeper:

### Working with the Panel

Statamatic - like other CMS - has a "Panel". That Dashboard enables you - or maybe the client/editor - to create and edit the contents easily in the Browser. You can create different users (admins) for the Panel. When first visiting the panel, locally, you are greeted to set up the first admin user. 

The panel users are by default NOT stored with Git. So your local users will not available on the fortrabbit App. You can now either include the users with Git by removing the according lines from `.gitignore` or - maybe better - set up the users locally and on the remote App:

* [{{app-name}}.frb.io/panel](https://{{app-name}}.frb.io/panel/) < address of the Panel


### Continuos development

We recommend to always develop locally first — it's just the most convenient way. Deploy, when you have reached a certain status of development. In many cases the real content will be created on the fortrabbit App. You can easily sync down changes from production into development with rsync.


### Backups

Some fortrabbit App plans have [backups enabled](/backups-uni). You can also spin your own solution. With Git you can already easily rollback changes. 


### Updating Statamatic

We recommend to update your local development environment first. On your local computer issue `composer update` in Terminal on root level of the project folder to trigger the update. When you have confirmed that everything works, `git push` to bring the latest updates to your fortrabbit App.


### Sending mails

fortrabbit does not support `sendmail`, so sending mails out of the box might not work. Statamatic provides config for transactional mail providers, or you can use a plugin to send e-mail over your own SMTP account.


### Getting a license

Remember that Statamatic is not a free software. Get a license, support the authors!