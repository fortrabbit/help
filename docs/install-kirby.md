---

template:         article
reviewed:         2024-02-26 08:36:48
title:            Install Kirby on fortrabbit
naviTitle:        Kirby
lead:             Kirby is a popular, file based CMS originated from Germany (YEAH!). Learn here how to install and tune Kirby on fortrabbit.
group:            Install_guides
stack:            uni
dontList:         false
workInProgress:   false

websiteLink:      https://getkirby.com/
websiteLinkText:  getkirby.com
category:         CMS
image:            kirby-mark.png
version:          4
supportLevel:     b

otherVersions:
    2 : install-kirby-2
    3 : install-kirby-3

keywords:
    - getkirby
    - starterkit
    - plainkit
    - flatfile
    - headless

---

## Get ready

Make sure you to have completed all steps in the [get ready guide](/get-ready).

### Choose your deployment work-flow

There are two "main" ways to deploy code here on fortrabbit: [Git](/git-deployment) and [SFTP](/sftp-uni). The general rule of thumb is, that content driven legacy applications, like [WordPress](/install-wordpress), are better uploaded in a classical manner with SFTP. Modern PHP web frameworks that are based on [Composer](/composer) are mostly deployed with Git. 

Now, Kirby is a bit in between and is - like [Grav](/install-grav) - file based. So there is no database, the actual contents are text files written on the file system.

[Our architecture graphic here](/deployment-methods-uni#toc-understanding-the-architecture) shows you that the files you can access via SFTP or SSH (Universal App) are not the ones that are in Git. The Git repo is a separate thing. So, deploying with Git is a one way street that only goes up, not down. In other words: You can NOT `git pull` any changes you have made on the App's file system. In an ideal world, code and content are maybe separated. With a file based CMS they are all together.

## SFTP upload

There is not much to say on that topic. Just make sure to upload all contents of your local Kirby folder, including the hidden `.htaccess` file into the `htdocs` folder within your App. Note that [Kirby does not let you set up the Panel by default on public-facing servers](https://getkirby.com/docs/guide/quickstart#create-your-first-account). If you upload your contents before setting up the panel and want to set up the Panel on the server, rather than locally, you need to [enable the `panel.install` option](https://getkirby.com/docs/reference/system/options/panel#allow-the-panel-to-be-installed-on-a-remote-server).

An alternative would be to use [rsync](/rsync) to upload/download everything.

## Install on the App via SSH

```shell
# 1. Login to your App via SSH
ssh {{ssh-user}}@deploy.{{region}}.frbit.com

# 2. Download latest Kirby starterkit
git clone https://github.com/getkirby/starterkit.git

# 3. Move unpacked files to top level
mv starterkit/* .

# 4. Remove the old folder
rm -rf starterkit

# 6. Use VIM to edit the config to enable the panel
vim site/config/config.php
```

To enable the panel you need in `site/config/config.php`:

```php
return [
  'panel' =>[
    'install' => true
  ]
];
```

Now you can visit [{{app-name}}.frb.io/panel](https://{{app-name}}.frb.io/panel/) to create your first admin user and play around with Kirby CMS. That hasn't covered how you deploy just yet. Usually we advice to start with a local development environment first and then deploy and sync that one.

## Deploy with Git and Composer

Kirby 3 has Composer support, so a clean way to deploy it is possible. Make sure to read the [Kirby meets Composer](https://getkirby.com/docs/cookbook/installation/composer) article from the official Kirby docs. Here is a basic rundown:

1. Initialize Kirby with Composer on your local machine
2. Deploy to fortrabbit using Git and Composer
3. Synchronize contents with rsync

### Install Kirby locally with Composer

```shell
# 1. Create a local (on your own computer) Kirby project folder with Composer:
$ composer create-project getkirby/plainkit {{app-name}}
```

NOTE that Kirby offers a "starterkit" (with some demo contents and a theme with some templates) and a "plainkit" with no contents at all (which is used here). Maybe you also have a project running locally and are just looking for ways to deploy that. Continue with the next steps.

### Configure Kirby for Git deployment

Open your local Kirby project folder with your text editor or IDE. Open the (hidden) `.gitignore` file at the top level to tell Git to ignore some folders. Add these three lines:

```shell
…
# Kirby itself
/kirby

# Composer dependencies
/vendor

# Stuff you are creating
/content
…
```

NOTE that the setting above will also keep the `/content` folder out of Git. This is our opinionated way of doing it. It will help keeping your repo tidy by separating code from content. But you will need to run dedicated rsync commands to deploy and update the "contents" (see below). You can also decide to not touch the `.gitignore` so that you can deploy everything with Git all together. Keep in mind that you can not pull in new contents from the fortrabbit App this way.

At this point you should be able to run the project in your local development environment already. We highly recommend to develop the site locally, use fortrabbit for staging and production.

### Deploy Kirby with Git

```bash
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

* [{{app-name}}.frb.io/panel](https://{{app-name}}.frb.io/panel/) < address of the Panel

### Synchronize your contents

As mentioned above, deployment of your code base (templates and configuration) and dependencies (Kirby and Composer) is done via Git deployment. Deploying the content is a separate step. We recommend to use rsync to upload or download new contents to and from your remote fortrabbit App (see also our [rsync article](/rsync)). On your local computer in the Terminal in the kirby project folder execute:

```shell
# SYNC UP: from local to remote
$ rsync -av ./content {{app-name}}@deploy.{{region}}.frbit.com:~
```

It works also the other way around. For example in a case, where you have done some edits online and want those changes to be reflected in your local development environment:

```shell
# SYNC DOWN: from remote to local
$ rsync -av {{app-name}}@deploy.{{region}}.frbit.com:~/content ./
```

You can also use [SFTP](/sftp) to synchronize the `content` folder.

## Tuning

By now, you have the Kirby installation running on your local machine and you can easily deploy it to your fortrabbit App. You can deploy code changes and update Kirby with Git. Additionally contents are synced down and up using rsync or Git. Let's get deeper:

### Working with the Panel

Kirby - like other CMS - has a "Panel". That Dashboard enables you - or maybe the client/editor - to create and edit the contents easily in the browser. You can create different users (admins) for the Panel. When first visiting the Panel, locally, you are greeted to set up the first admin user. 

The panel users are by default NOT stored with Git. So your local users will not available on the fortrabbit App. You can now either include the users with Git by removing the relevant lines from `.gitignore` or - maybe better - set up the users locally and on the remote App:

* [{{app-name}}.frb.io/panel](https://{{app-name}}.frb.io/panel/) < address of the Panel

### Continuous development

We recommend always developing locally first — it's just the most convenient way. Deploy when you have reached a certain state of development. In many cases the real content will be created on the fortrabbit App. You can easily sync down changes from production into development with rsync.

### Backups

Some fortrabbit App plans have [backups enabled](/backups-uni). You can also spin your own solution. With Git you can already easily roll back changes. 

### Updating Kirby

We recommend updating your local development environment first. On your local computer enter `composer update` in the terminal at the root level of the project folder to trigger the update. When you have confirmed that everything works, `git push` to push the latest updates to your fortrabbit App.

### Sending mails

fortrabbit does not support `sendmail`, so sending mails out of the box might not work. Kirby provides config for transactional mail providers, or you can use a plugin to send e-mail over your own SMTP account.

### Getting a license

Remember that Kirby is not free software. Get a license, support the authors!
