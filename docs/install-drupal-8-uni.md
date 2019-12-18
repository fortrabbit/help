---

template:         article
reviewed:         2017-12-20
title:            Install Drupal 8
naviTitle:        Drupal
lead:             Drupal 8 is one of the best known open source PHP CMS. Learn here how to use it with fortrabbit.
group:            Install_guides
stack:            uni
proLink:          install-drupal-8-pro

websiteLink:      https://www.drupal.org/?utm_source=fortrabbit
websiteLinkText:  drupal.org
category:         CMS
image:            drupal8-mark.png
version:          8.2
deprecated:       yes
dontList:         true
dontIndex:        true
supportLevel:     c


keywords:
    - drupal
    - install

---

## Get ready

We assume you've already created an App with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


## Quick start

Start by downloading the [latest Drupal archive](https://www.drupal.org/8) from the Drupal website and unpack it locally. It will extract into a folder named `drupal-8.2.4` - the name varies wit the actual version you are using. Now copy the **contents** of the local `drupal-8.2.4` folder (not the folder itself) via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When the upload is finished, visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in your browser and commence with the guided web installation to finish the setup. In the "Database configuration" form make sure the "advanced options" are opened, then enter:

* **Database type**: `MySQL, MariaDB, Percona Server, or equivalent`
* **Database name**: `{{app-name}}`
* **Database username**: `{{app-name}}`
* **Database password**: [Look up in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql)
* **Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Port number**: keep `3306`
* **Table name prefix**: keep empty (or adjust to your preferences)

Following enter your site name, admin user, e-mail and password. After the configuration is done, you will be redirect to your site - but it will look rather minimalistic, because none of the stylesheets are loaded. This is because Drupal uses to not supported directives in a `.htaccess` file. To finish up, please open the file `htdocs/sites/default/files/.htaccess` via SFTP or from SSH, then:

* Find the line reading `Options -Indexes -ExecCGI -Includes -MultiViews` (near the top)
* Remove `-ExecCGI` and `-Includes` so that it reads `Options -Indexes -MultiViews`
* Save

**Note**: If you edit the file from SSH and cannot safe it: execute `chmod u+w sites/default/files/.htaccess` before modifying the file and `chmod u-w sites/default/files/.htaccess` when you are done.

After that, the setup is done and you can browse to your freshly installed Drupal:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< Drupal installation_
* [{{app-name}}.frb.io/user/login](https://{{app-name}}.frb.io/user/login) _< Drupal admin_


## Advanced topics

See our Drupal install guide for the [Professional Stack](/app-pro) to learn more about these topics:

* [Drupal with Composer](/install-drupal-8-uni#toc-install)
* [Git Deployment](/install-drupal-8-uni#toc-configure-git)
* [Database migration](/install-drupal-8-uni#toc-mysql-import)


