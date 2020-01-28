---

template:         article
reviewed:         2020-01-28
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

First, make sure to enable the Object Storage component with your Pro App. This step is not required, if you did not disable it when creating your App.

Now you have access to our S3 compatible [Object Storage](/object-storage). You do not need an AWS account, since this is a first party service we provide.

To connect Craft with the Object Storage, install the [fortrabbit/craft-object-storage](https://plugins.craftcms.com/fortrabbit-object-storage) plugin we prepared for you. 
 On the [Github page](https://github.com/fortrabbit/craft-object-storage) of the plugin you find further setup the instructions.

Once the plugin is installed and enabled, a new Volume Type "fortrabbit Object Storage" is available. In your `config/volumes.php` you can setup additional volumes. 

To access the Object Storage from your computer, use a S3 compatible "SFTP" client - [Transmit for Mac](https://panic.com/transmit/) works best in our experience.


### Using the Craft Imager plugin

The [aelvan/Imager-Craft](https://github.com/aelvan/Imager-Craft) provides tons of options for image transforms including support for `webp`. Please mind we do not support any optimizers besides `webp`. If **not** using **imgix**, we suggest the following settings:

* `'imagerSystemPath' => '/tmp/imager'` To make sure cached transforms persist when you deploy new code.
* `'useCwebp' => true` Use the cwebp command line tool (ImageMagick works with webp as well).
* `'fillInterval' => 600` If you make use of fillInterval, make sure to increase the interval (defaults to 200) to avoid too many different image instances.



## Next steps

Continue with [tuning Craft](/craft-3-tune).
