---

template:         article
reviewed:         2022-05-25
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to the Object Storage with fortrabbit Professional Apps.
group:            craft
stack:            pro
dontList:         true

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          4.0
uniLink:          craft-assets-uni
supportLevel:     a

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-about) and have [Craft running on your local machine](/craft-install-local), [set up](/craft-setup) and [deployed](/craft-deploy-git). This guide is for advanced users on the advanced [Pro Stack](/app-pro).


## Craft CMS assets on Pro Apps

By default assets are stored in a local volume, which means user uploads are located in a folder along with the code. With Pro Apps you separate the uploads from your code, since your App usually runs on multiple nodes which do not share the same file system. Every time you deploy a new version of Craft or change templates, we replace the old with the new state. Read more about the nature of ephemeral storage [here](/app-pro#ephemeral-storage).


### Upload assets to the Object Storage

First, make sure to have the Object Storage Component enabled with your Pro App. That gives you access to our S3 compatible Object Storage. You don't need an AWS account, since this is a first party service we provide. Learn more about the fortrabbit Object Storage [here](/object-storage).

To connect Craft CMS with the Object Storage, install the [fortrabbit/craft-object-storage](https://plugins.craftcms.com/fortrabbit-object-storage) plugin. On the [GitHub page](https://github.com/fortrabbit/craft-object-storage) of the plugin you will find further setup instructions. As usual we expect this to be done locally first and then deployed via a `git push`.

Once the plugin is installed and enabled, a new Volume Type "fortrabbit Object Storage" is available. In your `config/volumes.php` you can set up additional volumes. 


### Accessing the Object Storage from your computer

See the [main Object Storage article](/object-storage#toc-object-storage-access) for more details on how to access the Object Storage from your computer.


## Next steps

Continue with [tuning Craft](/craft-tune).
