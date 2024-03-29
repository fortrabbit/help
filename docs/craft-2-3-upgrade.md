---

template:         article
reviewed:         2022-09-30 10:31:21
title:            Upgrade from Craft 2 to Craft 3
naviTitle:        Upgrade from Craft 2
lead:             What you need to know, when upgrading from a Craft CMS 2 installation to Craft CMS 3 here on fortrabbit. 
group:            craft
stack:            all
dontList:         true

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.7
supportLevel:     a

keywords:
  - craft
  - craftCMS2
  - craftCMS3
  - upgrade

---


## Get ready

First of all, please read the official upgrade guide: [docs.craftcms.com/v3/upgrade.html](https://docs.craftcms.com/v3/upgrade.html). Use that as your main source, your general guideline and single source of truth. Like with all our install guides, we assume you have a local development running where you will do most of the hard work. If not, see [here](/local-development). Also, as you will be using two Apps on fortrabbit, check out our [multi staging article](/multi-staging) for general concepts.

## About the changes in Craft 3

Craft 2 was way more simple. Craft 3 is different in many ways, modern and much more powerful. Just the way we like it: configuration with `dotenv` files, new directory structure, Composer based workflow and good Git support.

### Why to update Craft 2 to Craft 3

Craft CMS 2 is not supported by Pixel and Tonic any more. Craft CMS 2 is also not compatible with PHP 8. For PHP 7.4: End Of Live is November 2022. More and more hosting providers such as fortrabbit will stop supporting PHP 7.4.

### What about Craft 4

By the time of this writing, Craft 4 is already out. But Craft 3 is still supported. We suggest to update to Craft 3 first, since it's an easier path and more plugins are available. Do the Craft 3 to Craft 4 update later.

We also offer have an [upgrade guide](https://blog.fortrabbit.com/opinionated-craft-4-upgrade-guide) for that.

## Upgrade path

Approach this like you're building a new Craft 3 site. Make use of all the new features. Have a fresh start and a clean installation. Then import the contents from your old installation. This is your route:

### 1. Locally

The hard work is done in your local development environment:

1. Start a new Craft 3 project from scratch in a new folder
2. Import your templates, configs and contents from the old folder

So, at the end of this — ideally — you have two versions of your project/website running locally in parallel: the one based on Craft 2, and the new one on Craft 3. Both with the same content and probably the same look, but with a different engine.

### 2. On fortrabbit

On your fortrabbit production environment you are replicating your local workflow by creating two parallel versions:

1. Create a new App in the Dashboard.
2. [Deploy code with Git](/craft-deploy-git) and [rsync the assets](/craft-assets-uni) from your new local Craft 3 version to your new fortrabbit App.
3. Once you have two versions running locally and on fortrabbit smoothly, remove the [domain](/domains) from the old App running on Craft 2 and add it to the new App running on Craft 3. Check out our general [migration guide](/migrating) on how to switch seamlessly and without downtime.
4. After the domain switch is done, you can now safely delete the not needed Craft 2 fortrabbit App.

Don't worry about costs here too much. Remember that fortrabbit Apps are billed on a daily basis. So it will not burst your budget, when you have two Apps running here for a few days. And please don't hesitate to contact us when you hang somewhere.

## Further readings

There are other good articles on how to best update from Craft 2 to Craft 3:

* [Upgrad­ing from Craft 2 to Craft 3: Update Every­thing on Craft 2](https://straightupcraft.com/articles/upgrading-from-craft-2-to-craft-3-update-everything-on-craft-2) by Ben Parizek
* [Migrating a Craft 2 site to Craft 3](https://honcho.agency/migrating-a-craft-2-site-to-craft-3) by Michael Westwood at Honcho