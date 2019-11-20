---

template:         article
reviewed:         2019-09-30
title:            About Craft on fortrabbit
naviTitle:        About Craft on fortrabbit
order:            1
lead:             Craft 3 is the CMS you and your clients will love. We love it too. Our aim is to help you — the developer — to successfully develop and deploy Craft here. This is your entry point. 
group:            craft
stack:            all

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
  - Professional
  - Universal

---

## Choose your Stack

There are [two hosting stacks](/stacks) here on fortrabbit. You can choose with each new [App](/app) your create:

1. The [Universal Stack](/app-uni) has VPS or shared hosting feeling. It's easy to use, affordable, but limited in scaling. Use it for testing + small projects.
2. The [Professional Stack](/app-pro) is designed to scale horizontally. All components are redundant to assure high uptime. Use it for serious business + potentially large sites. 


## Choose your workflow

There are many ways to work with Craft CMS — depending on project needs and skills.

### Modern workflow

Sophisticated developers, experienced in backend development and ready to work with the Terminal, [Git](/git) and [Composer](/composer) will benefit the most from our advanced workflows. This is were our 💜 is.

1. **[Run Craft locally](craft-3-install-local)**: Install new with Composer or existing project
2. **[Setup Craft](/craft-3-setup)**: Prepare environments, sync database
3. **[Deploy Craft with Git](/craft-3-deploy-git)**
4. **Manage assets** by [rsync for Uni](/craft-3-assets-uni) or via [Object Storage for Pro](/craft-3-assets-pro)
5. **[Tune Craft](/craft-3-tune)**: Master it!


### Craft Copy

**Take a shortcut!** We have published this little handy open-source command line tool to speed up common deployment tasks around Craft CMS on fortrabbit. It connects your local Craft CMS installation with an App on fortrabbit and then enables you to sync database and assets in a speedy and convenient way. It guides you through the process of setting everything up, it even has a table to show you which parts are still missing. Give it a try! Here is an appetizer:

```bash
# Install and initialize
$ composer config platform --unset
$ composer require fortrabbit/craft-copy:^1.0.0-beta
$ ./craft install/plugin copy
$ ./craft copy/setup

# Sync database up (local ⟶ fortrabbit)
$ php craft copy/db/up
```

Please head on to the GitHub page for more usage examples:

* [github.com/fortrabbit/craft-copy](https://github.com/fortrabbit/craft-copy)


### Legacy workflow

Juniors and web developers with a focus on front-end are likely more comfortable using the legacy SFTP workflow. Use this workflow when you are coming from WordPress and not yet familiar to use the Terminal, Git and Composer. This is your route:

1. **[Run Craft locally](craft-3-install-local)**: Install new or existing project
2. **[Setup Craft](/craft-3-setup)**: Prepare environments, sync database
3. **[Upload Craft with SFTP](/craft-3-upload-sftp)**
4. **[Tune Craft](/craft-3-tune)**: Master it!
