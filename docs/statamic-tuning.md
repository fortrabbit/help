---

template:         article
reviewed:         2024-06-29 17:57:37
title:            Tuning Statamic
naviTitle:        Tune Statamic
order:            5
lead:             "By now, you hopefully have a Statamic installation running on your local machine and you can easily deploy it to your fortrabbit App. You can deploy code changes with Git and/or rsync. Let's dig deeper!"
group:            statamic
stack:            uni
dontList:         false

websiteLink:      https://statamic.com/
websiteLinkText:  statamic.com
category:         CMS
image:            statamic-mark-new.svg
version:          4
supportLevel:     b

keywords:
    - statamic
    - starterkit
    - markdown
    - laravel
    - flatfile
    - headless

---


## Adding domains

At the beginning you have probably been using the App URL as your domain. You can also add any number of domains to your App. Please see our [domain article](/domains) for more.

### ENV vars for Statamic MySQL setup

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

## Working with the Control Panel

Statamic - like other CMS's - has a control panel. That dashboard enables you - or maybe the client/editor - to create and edit the contents easily in the browser. You can create different users (admins) for the control panel.

* [{{app-name}}.frb.io/cp](https://{{app-name}}.frb.io/cp)

For this, please consider how you are dealing with the content created: 

* You should really not use the control panel on the App if you have all content stored in Git: you should only change things locally. 
* When using rsync (or SFTP) you can sync changes you have made on the App directly back down or the other way around.
* When using MySQL as a data store, you might want to dump the production database and bring it back locally from time to time

## Continuous development

We recommend to always develop locally first — it's just the most convenient way. Deploy when you have reached a certain state of development. In many cases the "real" content will be created on the fortrabbit App. You can easily sync down changes from production into development with rsync or sftp unless you have the content in MySQL.

## Backups

Some fortrabbit App plans have [backups enabled](/backups-uni). You can also spin your own solution. With Git you can already easily roll back changes. 

## Updating Statamic

We recommend updating your local development environment first. On your local computer execute `composer update` in the terminal at the root level of your project folder to trigger the update. When you have confirmed that everything works, `git push` to bring the latest updates to your fortrabbit App.

## Sending mails

fortrabbit does not support `sendmail`, so sending e-mails out of the box might not work. Statamic provides config for transactional mail providers, or you can use a plugin to send e-mail over your own SMTP account.

## Getting a license

Remember that Statamic is free for personal use. For a client project, get a license, support the authors!
