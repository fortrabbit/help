---

template:         article
reviewed:         2019-10-30
title:            Install Craft CMS 2 on fortrabbit
naviTitle:        Craft 2
group:            Install_guides
stack:            uni
proLink:          install-craft-2-pro

websiteLink:      https://craftcms.com/?utm_source=fortrabbit
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          2.6
group:            Install_guides
deprecated:       yes
dontList:         true

otherVersions:
    3 : craft-3-about


keywords:
    - craft
    - craftCMS
    - setup
    - install-guide

---

## Please note

**Craft 2 is deprecated.** Better not use it any more. It's end of life was/will be January 2020. See more details on the [official Craft CMS page](https://craftcms.com/guides/craft-cms-2-and-craft-commerce-1-end-of-life-information). We do have an [upgrade guide](/craft-2-3-upgrade).



## Get ready

We assume you've already created an App and chose Craft CMS in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard).

### Root path

If you haven't chosen Craft CMS stack when creating the App in the Dashboard, please set the following: Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


## Quick start

Start by [downloading the latest Craft release](https://craftcms.com/) from the Craft CMS website and unpack it locally. It will extract into multiple folders, so make sure that you unpack it in an empty parent directory. Now open the just extracted `craft/config/db.php` and change it as following:

```php
return [
	'server'      => getenv('MYSQL_HOST')     ?: 'localhost',
	'database'    => getenv('MYSQL_DATABASE') ?: 'local-db-name',
	'user'        => getenv('MYSQL_USER')     ?: 'local-db-user',
	'password'    => getenv('MYSQL_PASSWORD') ?: 'local-db-password',
	'tablePrefix' => 'craft',

	// to be compatible with MySQL 5.7, requires 2.6.2949 or greater
	'initSQLs'     => ["SET SESSION sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';"],

];
```

Before you upload anything: Rename the file `htaccess` in the extracted folder `public` to `.htaccess` (with a `.` in the beginning). If your operating system makes that hard for you, you can first upload and then rename the file directly on fortrabbit via SFTP.

Copy everything you just extracted, including the changed `db.php` file and the renamed `.htaccess` file, via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When the upload is finished, visit [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) in your browser and commence with the guided web installation to finish the setup.

After the guided web setup is done, you will be automatically redirected to the Craft CMS admin login form. Use the previously chosen Username and Password to login. Your Craft CMS is now live under 😋:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< Craft CMS installation_
* [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) _< Craft CMS admin_


## Advanced topics

See our Craft install guide for the [Professional Stack](/app-pro) to learn more about these topics:

* [Git Deployment](/install-craft-2-pro#toc-deploy-with-git)
* [Database migration](/install-craft-2-pro#toc-database-migration)
* [Mail delivery](/install-craft-2-pro#toc-sending-mail)



<!-- TODO: write on how to customize it -->
<!-- TODO: hint or example to install with wget via SSH  -->
