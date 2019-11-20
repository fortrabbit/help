---

template:         article
reviewed:         2019-03-29
title:            Install Grav
naviTitle:        Grav
lead:             Grav is a popular, free, file based CMS based on Twig & Markdown. Learn here how to install and tune Grav on fortrabbit.
group:            Install_guides

websiteLink:      https://getgrav.org?utm_source=fortrabbit
websiteLinkText:  getgrav.org
category:         CMS
image:            grav-symbol.png
version:          1.6
stack:            uni
proLink:          install-grav-1-pro

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready).

## Quick start

Start by downloading the "latest Grav Core + Admin archive" from the [Grav website](https://getgrav.org/downloads) from the Grav website and unpack it locally. It will extract into a folder named `grav-admin`. Now copy the **contents** of the local `grav-admin` folder (not the folder itself) via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When the upload is finished, visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in your browser and commence with the guided web installation to finish the setup. When that is done, your Grav installation is completed and you can visited it in the browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< Grav installation_
* [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) _< Grav admin_


## Advanced setup and migration

**Don't stop with a plain vanilla installation. Make it yours!** Check out the following topics if you have an existing Grav installation or if you would like to setup Grav so that you can run in a local development environment as well as in your fortrabbit App:

### Install plugins & themes

All the regular ways are supported: Use the Grav admin, use the command line or just unpack the plugins and themes to the appropriate directories.

### Environment configuration

Grav allows you to [configure different settings based on the domain](https://learn.getgrav.org/advanced/environment-config) it is served from. For example, if you are developing your Grav site locally, you probably want the debugging enabled while you want it to be disabled, when serving the App from fortrabbit.

To that end, you can create domain specific configurations. All you need to do is create a new `system.yaml` configuration file for you custom domain. Say the domain you are using is `www.my-grav.tld`, you would create the file `user/www.my-grav.tld/config/system.yaml`, in which you then set:

```yaml
debugger:
  enabled: false
```

An even better approach is set your `user/config/system.yaml` as restrictive as possible while using a special localhost configuration in `user/localhost/config/system.yaml`, which enables debugging and so on:

```yaml
debugger:
  enabled: true
```

## Alternative workflow with Git

You might be tempted to manage deploy Grav using Git. This is possible, but requires some planing ahead: As Grav is a file based CMS, code and content life very close together. It would be nice to have the content under version control as well, but what happens when you or the client is editing contents via the Admin plugin on the App directly? Remember: The [contents of the web storage are not the Git repo](deployment-methods-uni) and can not be synced back into it. So you can use these strategies:

### Don't generate any content on the App itself

This workflow is good when you — the developer - are using Grav for private development blog, or you collaborate with other developers who prefer to edit the contents directly in a text-editor locally. So you skip the admin plugin for good, keep Git and code in Git and deploy with Git to publish new articles.

### Keep the contents out of Git

You can also exclude the contents from Git. So you deploy only your theme and the templates by Git, while the contents can be edited on remote and synced via SSH or SFTP.

### Create a point of switch

During initial development, everything is in Git, including content. This allows to control the base structure and copy while deciding on how pages might be broken down, what copy belongs in what area etc.

Once it's ready to handover to the client, the user-editable content get's pulled out of Git. Simply leave it on the App's file system so it can be edited from that point on via Grav admin and not overwritten by Git. The fortrabbit backups are helping to keep things stored safely. 

The rest of Grav stays in Git. Theme, plugin or core Grav upgrades are first done in local dev environment, then committed, and deployed.