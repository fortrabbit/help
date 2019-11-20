---

template:         article
reviewed:         2019-09-30
title:            Upload Craft CMS with SFTP 
naviTitle:        Upload Craft with SFTP
lead:             Are you more "web designer" and less a "web developer"? Learn how to upload Craft in a classical way using SFTP. 
group:            craft
stack:            uni
dontList:         true

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          3.3

otherVersions:
    2 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


This legacy guide here follows the easiest path to get Craft up and running on fortrabbit.  We also have a more advanced workflow to [deploy Craft with Git](/craft-3-deploy-git).

## Get ready

In any case, you should have [local development environment](local-development) and [Craft installed](/craft-3-install-local) and [configured](/craft-3-setup), also see our [get ready guide](/get-ready).

## Upload Craft with SFTP

This workflow is simple and common. It doesn't need much explanation. Everybody and his dog knows [how to use SFTP](/sftp). Check the [downloading an archive file manually](https://docs.craftcms.com/v3/installation.html#downloading-an-archive-file-manually) workflow from the official Craft docs as your detailed reference. 

On the fortrabbit side: Just grab your personal SFTP login credentials from the Dashboard. Use any SFTP client. Upload all contents of your local Craft folder into the `htdocs` folder of your fortrabbit App. And you are good to go.

## Troubleshooting

Doesn't work as expected? Keep calm and read on:

### Connection error

**Got an error while connecting with SFTP?** Doing this for the first time on fortrabbit? Please see the [access troubleshooting](/access-methods#toc-troubleshooting) and our [SFTP guide](/sftp) to get started and all setup.

### File permissions

You might need to change file permissions. Make sure that Craft can write the files `composer.json`, `composer.lock`, `storage/*` and `vendor/*` on the App. Set the file permissions for those files to `744` with your SFTP client. 

### Hidden .htaccess file

Don't forget to upload hidden `web/.htaccess` file. This file is required. You can not see that file in your Desktop, unless you set the option to show hidden files. The file browser from your SFTP client most likely will show that file by default. Leave the other hidden `.env` file — which is only for your local development — at home.

### Service unavailable error

When you don't have a local development environment like suggested and just upload the latest .zip package from Craft via SFTP (and not made any of the errors above), it will not work out of the box and throw the service unavailable error. Within the logs you can see that the error was caused in line 515 in `Application.php`. When you look at the lines before in that file you can see that a condition for the installer to run is, that it has to be in "dev" mode.

**To fix that**: Change the ENV var in the Dashboard from: `ENVIRONMENT=production` to `ENVIRONMENT=dev`

Run the installer like so (the base URL will still throw an error):  
[{{app-name}}.frb.io/admin/](https://{{app-name}}.frb.io/admin/)

You can then change the ENV back to production. We actually assume that the Craft you have on fortrabbit is the production and that all development is done locally. Please read our guides!


## Consider

A downside of the SFTP workflow is, that you have keep both of your Craft environments in sync manually. Our [manage assets with rsync](/craft-3-assets-uni) is optional but still helpful for SFTP users.


## Next steps

Your Craft fortrabbit App should already connect to the fortrabbit database, thanks to the [Software Preset](/app#toc-software-preset). Next, [configure Craft](/craft-3-setup) to complete your setup.
