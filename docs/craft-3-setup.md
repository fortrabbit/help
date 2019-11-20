---

template:         article
reviewed:         2018-11-19
title:            Setup Craft CMS
naviTitle:        Setup Craft
lead:             How to configure Craft CMS to run locally AND on fortrabbit.
group:            craft
stack:            all
order:            10

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          3.3

otherVersions:
    2 : install-craft-2-uni


keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local) and deployed it your fortrabbit App. 

## Craft configuration on fortrabbit

Craft 3 uses modern `.env` style configuration, learn more about the concepts [here](/env-vars). In result, you can run your Craft locally and on remote without code or configuration file changes. Locally, your `.env` file will be modified and read.


## Security key

The mandatory Craft CMS security key has to be shared among all environments. We recommend to use your local security key as the master key. When you used Composer to install, that key was shown at the end of the installation. If not, open your local (hidden) `.env` file from the root folder of your project and find a line that looks like this:

```dotenv
SECURITY_KEY=69UzZSEquw9E7RdCyRRTRb1lxe7h0EPd
```

It will contain a value when you have [installed Craft 3 correctly on your local machine](/craft-3-install-local). Copy that line. Go to the App's ENV vars settings in the Dashboard and paste that line. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

That ENV var is already set. Just replace it with your local one. Also see the [official Craft guide on that topic](https://docs.craftcms.com/v3/installation.html#step-3-set-a-security-key) to learn about the different ways to create the key.


## Database setup

TLDR: **No need to configure the MySQL database connection for fortrabbit, it should be set.** On fortrabbit the [environment variables](/env-vars) are getting seeded from the ones set in the Dashboard (not from the .env file). When you have chosen Craft in the [Software Preset](/app#toc-software-preset) while have creating the App, all ENV vars at fortrabbit are already pre-populated. If not, see [here](craft-3-tune#toc-manually-set-env-vars).


## Database synchronization

Now, your [local Craft installation](/craft-3-install-local) should already have created a MySQL database with a few tables in it. The fortrabbit database, on the other side, is still empty. Now, export your local database and import it to the fortrabbit remote. Head over to our [MySQL export & import guide](/mysql#toc-export-amp-import) to learn how to access the database on fortrabbit and export/import tables.

**PRO TIP**: You will probably often synchronize development and production databases. We have developed a handy command line tool: **[Craft Copy](https://github.com/fortrabbit/craft-copy)** to speed that up. It works like this:

```bash
# Sync database up (local ⟶ fortrabbit)
$ php craft copy/db/up

# Sync database down (local ⟵ fortrabbit)
$ php craft copy/db/down
```

## Next steps

Craft CMS is configured to run locally and is also ready for fortrabbit. Next you can [deploy it with Git](/craft-3-deploy-git) or [upload it by SFTP](/craft-3-upload-sftp). Don't forget our [Craft tuning guide](/craft-3-tune) afterwards.
