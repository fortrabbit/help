---

template:         article
reviewed:         2023-03-07
title:            Statamic on fortrabbit
naviTitle:        Statamic intro
order:            1
lead:             Statamic is a cool file based CMS. Learn here how to install and tune Statamic on fortrabbit.
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

Make sure you to have completed all steps in the [get ready guide](/get-ready). Have a local development environment with PHP and a web server ready. Also have an App at fortrabbit ready. There is no Statamic preset yet, so choose "Laravel" (since Statamic is based on Laravel) when booking a new App in the fortrabbit Dashboard.

## Install Statamic locally

Before you deploy anything to fortrabbit, get your Statamic project running on your local computer. Please see the [official Statamic install guide](https://statamic.dev/installing/local) for up-to-date instructions. Also see our [local development article](/local-development) for options setting up a local development environment. It would be good to already have your first admin user set up.

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

## Deployment options

Here are three ways to get your code up (and down):

### 1 - SFTP upload

There is not much to say on that topic. Just make sure to upload all contents of your local Statamic folder, including the hidden `.htaccess` file into the `htdocs` folder within your App.

Consider using [rsync](/rsync) to sync all contents all up and down.

### 2 - Git + rsync workflow (recommended)

Manage template code with Git and content with rsync. Instructions [over here](/statamic-git-rsync).

### 3 - Git experimental workflow

Feeling adventurous? Use Git on the App to be able to push/pull contents. Instructions [over here](/statamic-git-experimental).