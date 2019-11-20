---

template:         article
reviewed:         2019-09-30
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to the Object Storage with fortrabbit Professional Apps.
group:            craft
stack:            pro
workInProgress:   yes

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          3.3
uniLink:          craft-3-assets-uni

otherVersions:
    2.6 : install-craft-2-pro

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-3-about) and have [Craft running on your local machine](/craft-3-install-local), [setup](/craft-3-setup) and [deployed](/craft-3-deploy-git). This guide is for advanced users on the advanced [Pro Stack](/app-pro).

## About Craft CMS assets

With Craft 3, the "assets" folder contains files that are managed by the CMS. This is the user generated stuff, uploaded files, mostly images — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. 

The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to an Pro App will get destroyed the next you time deploy anyways, see [here](/app-pro#ephemeral-storage) for more and why.

### Upload assets to the Object Storage

So what you want: is to swap the assets folder on the file system with external files stored on the fortrabbit [Object Storage](/object-storage). We have developed a Craft plugin to connect the fortrabbit Craft App with the Object Storage. All uploads will transferred to the Object Storage directly, all URLs will point to the Object Storage. See the install guide on GitHub for usage:

* [github.com/fortrabbit/craft-object-storage](https://github.com/fortrabbit/craft-object-storage)

Once the plugin is installed and enabled, a new Volume Type "fortrabbit Object Storage" is available. In your `config/volumes.php` you can setup additional volumes. 

To access the Object Storage from your computer, use a S3 compatible SFTP client - [Transmit for Mac](https://panic.com/transmit/) works best in our experience.

## Next steps

Continue with [tuning Craft](/craft-3-tune).
