---

template:         article
reviewed:         2020-08-26
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to Universal Apps using rsync or SFTP.
group:            craft
stack:            uni

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.6
proLink:          craft-3-assets-pro
supportLevel:     a

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-3-about). Further, this guide is for advanced users on the [Uni Stack](/app-uni) already using [Git deployment to deploy Craft](/craft-3-deploy-git). You don't need to care about all this, when you have [uploaded Craft with SFTP](/craft-3-upload-sftp).


## About Craft Assets

Craft CMS "assets" are user generated stuff: uploaded files, mostly images that lives in "volumes" — also see [the official Craft docs](https://craftcms.com/docs/3.x/assets.html) on that. 

When creating asset volumes in the Craft CMS control panel, their contents are by default excluded from Git, so that code and content are separated. However, if you've already created a directory in your `web` folder and you now reference that directory as the webroot of a volume you're creating, you will need to manually configure Git to ignore the files in it. To do this, add a file `.gitignore` in the volume's directory, with the following contents:

```
*
!.gitignore
```

Another reason that assets should be managed and deployed independently from Git deployment, is that files uploaded to the production App would [not be represented in Git](https://help.fortrabbit.com/deployment-methods-uni#toc-git-works-only-one-way) anyway.


## Options to sync assets

There are two directions you want your assets to flow:

* UP — When you first developed Craft locally, you may want to upload all the images you already have locally to the fortrabbit App; 
* DOWN — When the App is running in production for some time and the clients or editors have created content on the App you want to download that, so your local version is in sync.

Here are your options for doing this with a Universal App:


### 1 — Use Craft Copy

We have developed a little command line tool to help with common deployment tasks around Craft CMS here on fortrabbit. Beside other tasks it also helps with the assets. See the official docs on how to install and use:

* [github.com/fortrabbit/craft-copy](https://github.com/fortrabbit/craft-copy)


### 2 — Manage assets with rsync

Give the good old `rsync` command line tool a try. It's easier than you think: 

```
# Sync UP local assets with remote
$ rsync -av ./web/{your-asset-folder} {{app-name}}@deploy.{{region}}.frbit.com:~/web/
```

Replace the path and your individual folder name with your own settings. Our [rsync tutorial](/rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.


### 3 — Manage assets with SFTP

Good old [SFTP](/sftp-uni#toc-accessing-sftp) is another valid way to to do it. Just fire up your SFTP client, log in to the App and manage the assets manually. Many SFTP clients are offering sync options, to keep local and remote files up-to-date.  


### 4 — Outsource assets to a cloud storage

This is basically what we are doing with [Object Storage](/object-storage) for the [Pro Apps](/app-pro): The assets are not stored on the file system of the App any more; once the user uploads files, they get uploaded to external cloud storage like S3 (or the Object Storage here). Then, within the templates you hot-link all the files to the external storage. This more professional design helps to reduce load on the App. Look out for S3 Craft plugins to get started with this. Only use this if you really need it. Don't over-engineer.


## Next steps

Check out how to [tune](/craft-3-tune) and [update](/craft-3-update) your Craft installation.
