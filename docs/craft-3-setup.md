---

template:         article
reviewed:         2021-04-13
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
version:          3.6
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

Craft 3 uses modern `.env` style configuration, learn more about the concepts [here](/env-vars). This means you can run your Craft locally and remotely without code or configuration file changes. Locally, your `.env` file will be modified and read.


## Security key

The mandatory Craft CMS security key has to be shared among all environments. We recommend to use your local security key as the master key. When you used Composer to install, that key was shown at the end of the installation. If not, open your local (hidden) `.env` file from the root folder of your project and find a line that looks like this:

```dotenv
SECURITY_KEY=69UzZSEquw9E7RdCyRRTRb1lxe7h0EPd
```

It will contain a value if you have [installed Craft 3 correctly on your local machine](/craft-3-install-local). Copy that line. Go to the App's ENV vars settings in the Dashboard and paste that line. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

That ENV var is already set. Just replace it with your local one. Also see the [official Craft guide on that topic](https://docs.craftcms.com/v3/installation.html#step-3-set-a-security-key) to learn about the different ways to create the key.


## Database setup

TLDR: **No need to configure the MySQL database connection for fortrabbit, it should already be set.** On fortrabbit the [environment variables](/env-vars) are seeded from the ones set in the Dashboard (not from the .env file). If you chose Craft in the [Software Preset](/app#toc-software-preset) when creating the App, all ENV vars at fortrabbit will already be pre-populated. If not, see [here](craft-3-tune#toc-manually-set-env-vars).


## Next steps

Craft CMS is configured to run locally and is also ready for fortrabbit. Next you can [deploy it with Craft Copy](/craft-3-deploy-craft-copy).  Don't forget our [Craft tuning guide](/craft-3-tune) afterwards.