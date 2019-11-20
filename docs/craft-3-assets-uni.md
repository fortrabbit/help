---

template:         article
reviewed:         2019-09-30
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to Universal Apps using rsync or SFTP.
group:            craft
stack:            uni

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          3.3
proLink:          craft-3-assets-pro

otherVersions:
    2.6 : install-craft-2-pro

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-3-about). Further, this guide is for advanced users on the [Uni Stack](/app-uni) already using [Git deployment to deploy Craft](/craft-3-deploy-git). You don't need to care about all this, when you have [uploaded Craft with SFTP](/craft-3-upload-sftp).

## About Craft assets

With Craft 3, the `assets` folder contains files that are managed by the CMS. This is the user generated stuff, uploaded files, mostly images — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. 

The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to the App are [not represented in Git](https://help.fortrabbit.com/deployment-methods-uni#toc-git-works-only-one-way) anyways.

So when you deploy using Git here, the assets must be managed and deployed independently. There are two directions: UP — When you first developed Craft locally, you may want to upload all the images you already have locally to the fortrabbit App; DOWN — When the App is running in production for some time and the clients or editors have created content on the App you want to download that, so your local version is in sync.

Here are your options for doing this with a Universal App:

## Manage Craft assets with rsync

We recommend giving `rsync` on the command line a try. It's easier than you think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

Our [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.

## Manage Craft assets with SFTP

Good old [SFTP](/sftp-uni#toc-accessing-sftp) is another valid way to to do it. Just fire up your SFTP client, login to the App and manage the assets manually. Many SFTP clients are offering sync options, to keep local and remote files up-to-date.  

## Outsource assets to a cloud storage

This is basically what we are doing with the [Object Storage](/object-storage) for the [Pro Apps](/app-pro): The assets are not stored on the file system of the App any more, once the user uploads files, they get uploaded to an external cloud storage like S3 (or the Object Storage here), within the templates you hot-link all the files to the external storage. This more professional design helps to reduce load on the App. Look out for S3 Craft plugins to get started with this. Use this, when you really need it. Don't over-engineer.

## Next steps

Make sure to have your [Craft setup](/craft-3-setup) correctly. Then [tune and optimize](/craft-3-tune).
