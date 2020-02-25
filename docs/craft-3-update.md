---

template:         article
reviewed:         2020-02-25
title:            Update Craft CMS
naviTitle:        Update Craft
lead:             We recommend to always use the latest version of all software for security reasons. Mind that you are responsible for the software you write yourself and use. Here are some strategies to best keep Craft CMS up-to-date.
group:            craft
stack:            all
order:            20

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          3.4
supportLevel:     a

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

 

### Update your local development environment first

**Remember**: your local development environment is the master where changes are applied and tested first. Please update your local Craft CMS installation first and apply updates by deploying to production as a second step. Make sure that your environments are in sync that way.

For your production environment (fortrabbit App) we have set the ENV var "allowUpdates" to false (see above). That will make the update button not show up in the control panel on the App, so that your client will not accidentally update. It's a common mistake we see. Make sure your environments are in sync and you don't mess with updates and installing plugins on the App itself.

Here are three options to update Craft CMS and Craft plugins. Make sure to also check out the [official guides on the topic](https://docs.craftcms.com/v3/updating.html).


### A) Update Craft using the Control Panel – simple

1. Login to the Craft CMS Control Panel of your local Craft installation
2. Go to Utilities > Updates
3. Click the "Update all" button

This will update your local development environment to the latest versions of Craft CMS as well as all plugins. After testing changes, deploy updates to your production App (see below).


### B) Update Craft using the Craft CLI – advanced

Run the following command in the terminal on your computer **locally** in the root folder of the Craft project:

```bash
$ ./craft update
```

### C) Update Craft using Composer – super advanced

Run the following command in the terminal on your computer **locally** in the root folder of the Craft project:

```bash
$ composer update
```

**Please mind**: The two other options to update Craft above (via Control Panel or Craft CLI) are "pinning" the installed versions in `composer.json`. So instead of `"craftcms/cms": "^3.5.4"` you might just find `"craftcms/cms": "3.5.4"`. Best avoid mixing update methods. Running `composer update` after you have used the Craft CLI or the Control Panel will have no effect, since the versions in `composer.json` are fixed.


### Deploying Craft updates

After you have tested the updates locally you can deploy them to your App at forttrabbit. Here are your options:


#### A) Deploying updates with Git

1. Add and commit the local changes (`composer.json`, `project.yml` …)
2. Deploy changes by `git push` to your fortrabbit App

During the [Git deployment](/git-deployment) `composer install` will run automatically. This way your local Composer changes get applied on the remote. 


#### B) Deploying updates with SSH/SFTP based workflows

Continuous development with a SFTP workflow is a hustle. One strategy is to upload the changes files from local to the App. Another strategy is to re-run the very same steps that have been done locally on the App itself. Good luck.


### Database migrations

In many update cases a database migration is required. This will update the database table structure to match the latest updates. Sometimes it's a required extra step to be done with the App. We recommend to use `projectConfig` which will store all essential settings in a `project.yml` file. Here are your options to run migrations:


#### A) Trigger a database migration by visiting the Control Panel URL

After applying updates on your fortrabbit Craft App, you might see a "Service unavailable" message. Visit the control panel URL ([{{app-name}}.frb.io/cp](https://{{app-name}}.frb.io/cp) or whatever you have set) and you might see a message saying that database changes need to applied. Click the "apply" button to run the migration manually. 


#### B) Manually run database migrations on the App via Craft CLI

Instead of visiting the control panel URL you can trigger the changes via the Craft CLI in the terminal. The following commands are using remote SSH execution and are issued from your local computer:

<!-- @Oli: I think this is not safe for non SSH-key access ??? -->

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft migrate/all"
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft project-config/sync"
```

You can also login by SSH and call the commands on the App directly.


#### C) Automate database migrations

You can automate database migrations to be run every time you'll deploy. Options:


##### C.1) Auto-migrations with Craft Auto Migrate plugin

We created a little package ([see it on GitHub](https://github.com/fortrabbit/craft-auto-migrate)) which triggers `migrate/all` and `project-config/sync` every time you push changes.

```bash
# Install the craft-auto-migrate package (locally first)
$ composer require fortrabbit/craft-auto-migrate
```

With that in place, every time you deploy your code, database changes will be applied immediately. If no database changes are required, nothing happens, so it is safe to run all the time. Just make sure to test your upgrades and migrations locally first.


##### C.2) Auto-migrations with Craft Copy plugin

Craft auto-migrate is also included in our [Craft Copy](https://github.com/fortrabbit/craft-copy) (fortrabbit deployment tool plugin), so when you have that installed, a database migration will always run on `git push`.


##### C.3) Auto-migrations with Composer

You can also trigger the Craft CLI from your `composer.json` file 

```json
"scripts": {
    "post-update-cmd": [
        "./craft migrate/all"
    ]
}
```