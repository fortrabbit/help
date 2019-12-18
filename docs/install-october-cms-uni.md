---

template:         article
reviewed:         2017-12-20
title:            Install October CMS on fortrabbit
naviTitle:        October CMS
lead:             October is a free, open-source, self-hosted CMS based on the Laravel PHP framework. Learn how to install and use it on fortrabbit.

group:            Install_guides
stack:            uni
proLink:          install-october-cms-pro
deprecated:       yes
dontList:         true

websiteLink:      https://octobercms.com/?utm_source=fortrabbit
websiteLinkText:  octobercms.com
category:         CMS
image:            october-cms-mark.png
supportLevel:     c

keywords:
    - cms
    - install
    - laravel

---


## Get ready

We assume you've already created an [App](/app) and chose Custom in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard).

<!--

TODO: Frank: what about root path here? in the pro article we say to set the root path. In some other Uni articles we also do. Can't see a pattern here!

-->

## Quick start

Start by downloading the [latest October CMS archive](http://octobercms.com/download) from the October CMS website and unpack it locally. It will extract into the folder `install-master`. Now copy the **contents** of the local `install-master` folder (not the folder itself) via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When the upload is finished, visit [{{app-name}}.frb.io/install.php](https://{{app-name}}.frb.io/install.php) in your browser and commence with the guided web installation to finish the setup. You will need MySQL access, which is:

* **Database Type**: keep `MySQL`
* **MySQL Host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_
* **MySQL Port**: keep `3306`
* **Database Name**: `{{app-name}}`
* **MySQL Login**: `{{app-name}}`
* **MySQL Password**: [Look up in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql)

After the guided web setup is done, you should first clean up the installer files. Login again via SFTP and remove:

* `installer.php`
* `install_files` (directory)

Now you can proceed to your freshly installed October CMS. It is now live under:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< October CMS installation_
* [{{app-name}}.frb.io/backend](https://{{app-name}}.frb.io/backend) _< October CMS admin_
