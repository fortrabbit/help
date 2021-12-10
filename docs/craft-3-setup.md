---

template:         article
reviewed:         2021-12-10
title:            Setup Craft CMS
naviTitle:        Setup Craft
lead:             How to configure Craft CMS to run locally AND on fortrabbit.
group:            craft
stack:            all
order:            10

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.7
supportLevel:     a


keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local) and deployed it your fortrabbit App. 


## Craft configuration on fortrabbit

Craft 3 uses modern `.env` style configuration, learn more about the concepts [here](/env-vars). This means you can run your Craft locally and remotely without code or configuration file changes. Locally, your `.env` file will be modified and read. That file is not part of Git, you don't need it on the App.


## Database setup

**No need to configure the MySQL database connection for fortrabbit.** On fortrabbit the [environment variables](/env-vars) are seeded from the ones set in the fortrabbit Dashboard (not from the .env file). If you chose Craft in the [Software Preset](/app#toc-software-preset) when creating the App, all ENV vars at fortrabbit will already be pre-populated. If not, see [here](craft-3-tune#toc-manually-set-env-vars).


## Next steps

Craft CMS is configured to run locally and is also ready for fortrabbit. Next you can [deploy it with Craft Copy](/craft-3-deploy-craft-copy).  Don't forget our [Craft tuning guide](/craft-3-tune) afterwards.