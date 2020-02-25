---

template:         article
reviewed:         2020-02-10
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to Universal Apps using rsync or SFTP.
group:            craft
stack:            uni

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.4
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

With Craft CMS "assets" is user generated stuff: uploaded files, mostly images that lives in "volumes" — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. 

Files within local "asset" "volumes" are by default excluded from Git, so that code and content are separated. Further more, files uploaded to the App directly would [not be represented in Git](https://help.fortrabbit.com/deployment-methods-uni#toc-git-works-only-one-way) anyways. So when you deploy Craft CMS using Git, the "assets" must be managed and deployed independently.


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
$ rsync -av ./web/{your-asset-folder} {{app-name}}@deploy.{{region}}.frbit.com:~/web/{your-asset-folder}
```

Replace the path and your individual folder name with your own settings. Our [rsync tutorial](/rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.


### 3 — Manage assets with SFTP

Good old [SFTP](/sftp-uni#toc-accessing-sftp) is another valid way to to do it. Just fire up your SFTP client, login to the App and manage the assets manually. Many SFTP clients are offering sync options, to keep local and remote files up-to-date.  


### 4 — Outsource assets to a cloud storage

This is basically what we are doing with the [Object Storage](/object-storage) for the [Pro Apps](/app-pro): The assets are not stored on the file system of the App any more, once the user uploads files, they get uploaded to an external cloud storage like S3 (or the Object Storage here), within the templates you hot-link all the files to the external storage. This more professional design helps to reduce load on the App. Look out for S3 Craft plugins to get started with this. Use this, when you really need it. Don't over-engineer.


## Next steps

Check out how to [tune](/craft-3-tune) and [update]([tune](/craft-3-update)) your Craft installation.
