---

template:         article
reviewed:         2020-01-20
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to the Object Storage with fortrabbit Professional Apps.
group:            craft
stack:            pro

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.3
uniLink:          craft-3-assets-uni
supportLevel:     a

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


By default assets are stored in a local volume, which means user uploads are located in a folder along with the code. 

With Pro Apps you separte the uploads from your code, since your App usually runs on multiple nodes which do not share the same file system. Every time you deploy a new version of Craft or change templates, we replace the old with new state. Read more about nature of the  
 [Ephemeral storage](/app-pro#ephemeral-storage).


### Upload assets to the Object Storage

So what you want: is to swap the assets folder on the file system with external files stored on the fortrabbit [Object Storage](/object-storage). We have developed a Craft plugin to connect the fortrabbit Craft App with the Object Storage. All uploads will transferred to the Object Storage directly, all URLs will point to the Object Storage. See the install guide on GitHub for usage:

* [github.com/fortrabbit/craft-object-storage](https://github.com/fortrabbit/craft-object-storage)

Once the plugin is installed and enabled, a new Volume Type "fortrabbit Object Storage" is available. In your `config/volumes.php` you can setup additional volumes. 

To access the Object Storage from your computer, use a S3 compatible SFTP client - [Transmit for Mac](https://panic.com/transmit/) works best in our experience.



## Next steps

Continue with [tuning Craft](/craft-3-tune).
