---

template:         article
reviewed:         2022-03-21 15:50:46
title:            Deploy Craft CMS with Craft Copy
naviTitle:        4. Deploy Craft
lead:             Learn how to deploy Craft CMS code base with Git to fortrabbit. 
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.7
supportLevel:     a
order:            6

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---



## Get ready

Make sure to have completed all steps from the [get ready guide](/craft-3-about). You now should have Craft installed in your local development environment already, if not see [here](craft-3-install-local).


## About Craft Copy

Craft Copy is a command line tool to help you deploying Craft CMS from our local development environment to your fortrabbit App. It will take care of the code, the assets and the database.

<div class="responsive-video">
  <iframe src="https://www.youtube-nocookie.com/embed/h8g5t-X6ya4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>



## Installing and using Craft Copy

Please see the **[Craft Copy usage guide at GitHub](https://github.com/fortrabbit/craft-copy#requirements)**. It guides you through the process of installing the plugin, connecting your App, deploying code, assets and database.


## Alternative ways to deploy Craft CMS

Don't feel like using Craft Copy? Np problem! Here are some alternative ways to deploy Craft on fortrabbit:

### SFTP deployment

There is a more basic guide: [Install Craft using SFTP](/craft-3-upload-sftp).

### Git deployment

If you prefer to use Git directly, check out our guide: [Deploy Craft CMS with Git](/craft-3-deploy-git)


### Assets management

There are two dedicated guides here to deploy the Craft `assets` folder separately. It depends on your [App's Stack](/craft-3-about#toc-1-1-choose-your-stack): 

* For Universal Apps: [Deploy assets with rsync](/craft-3-assets-uni)
* For Professional Apps: [Deploy assets to the Object Storage](/craft-3-assets-pro).


### Database 

Please see our general [MySQL guide](/mysql) for various ways to how to copy the database from local to remote and the other way around.


## Next steps

See our [Craft tuning guide](/craft-3-tune) to truly master Craft on fortrabbit.
