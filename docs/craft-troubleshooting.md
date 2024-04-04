---

template:         article
reviewed:         2024-04-04 12:21:13
title:            Troubleshooting Craft CMS issues
naviTitle:        Craft troubleshooting
lead:             Tips, tricks, best practices and advanced topics on how to run Craft CMS successfully on fortrabbit.
group:            craft
stack:            all
order:            110

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          5.0
supportLevel:     a

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

rank: 190

---


## You see a 504 message

That's a timeout issue. Commonly this happens when Craft is busy processing the queue or doing too many image transformations at once or something else is blocking. Please see our [Craft CMS performance article](/craft-performance) for more details on the matter.

## You see a Max Execution Time warning

The Craft CMS Control Panel has a system report that will check for technical requirements. Our default setting for `max_execution_time` is 60 seconds. The Control Panel will complain that: "Craft requires a minimum PHP max execution time of 120 seconds ... "

The truth is, only heavy tasks like updates, backups and queue execution in the Craft Control Panel may take longer than 60 seconds. For site delivery a short `max_execution_time` is beneficial to prevent blocking PHP resources. No HTTP response should take longer than a few seconds, actually.

Please also see our [Craft CMS performance article](/craft-performance) for more details on the matter.

## You see a "Service Unavailable" or 503 message

What to do when your Craft CMS throws an "Service Unavailable" message on the screen instead of rendering your website? Don't be afraid, you can likely solve that yourself. Especially during setup it's likely that some tiny config is still missing or wrong. Check the logs.

### Common errors with initial setup

Here are some common errors, the cause of 90% of failing Craft CMS installations here:

* **Mismatching Security Key** — Make sure to have the same key with your local development environment and on your fortrabbit App; see our [security key setup guide](craft-setup#toc-security-key).
* **Wrong database configuration** - Make sure your fortrabbit database is using the ENV vars provided by the fortrabbit Dashboard to connect to the fortrabbit database in production. Leave your `.env` file at home as it will be ignored anyway. See our [environment variables article](/env-vars) as well. 
* **Missing table prefix** - You might have a table prefix locally, some users do this locally to have multiple installations in one big database. To distinguish between them you can set up a table prefix. You can do this with an environment variable: locally you set that with your `.env` file, in fortrabbit via the Dashboard.
* **Missing .htaccess file** — Commonly happens with SFTP upload. The `.htaccess` file is hidden, make sure to copy it over as well.

#### Temporarily turning on/off dev mode

Your fortrabbit App is set to be your production environment per default. So when an runtime exception occurs, you will presented with the very basic "Service Unavailable" error screen. This is to prevent visitors of your website from seeing any information about the system and the errors. One thing you can do, is temporarily setting DevMode to true, so that you can see the error output printed on screen. We advise against doing it, unless it is a non-production environment. 

## Large assets upload problems

Most likely this is a setting within Craft CMS itself. The setting you are looking for is `maxuploadfilesize` and its default is 16MB, please see the official [Craft Docs on that](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-maxuploadfilesize). This can also be caused by the `post_max_size`, `memory_limit`, `upload_max_filesize` or `max_execution_time` settings, which you can configure in the Dashboard, but by default those are OK. If that still doesn't help, check the [logs](/logging-uni) to see if you can find some meaningful errors.
