---

template:         article
reviewed:         2019-09-03
title:            Deploy Craft CMS with Git 
naviTitle:        Deploy Craft with Git
lead:             Learn how to deploy Craft CMS code base with Git to fortrabbit. 
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark.svg
version:          3.3

otherVersions:
    2.6 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---



## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-3-about), have [Craft installed locally](craft-3-install-local) and [setup](/craft-3-setup). This guide is for advanced users, making use of [Git](/git) and [Composer](/composer), it can be applied to [Professional](/app-pro) and [Universal Apps](/app-uni) on fortrabbit. There is a more basic guide to install Craft using SFTP [over here](/craft-3-upload-sftp).


<!--
Git deployment related?
## The storage folder

The `storage` folder within Craft is part of the [fortrabbit custom `.gitignore` file](). 
-->

## Deploy the Craft code base with Git

Trigger the following commands in your **local** terminal:

```
# 1. Initialize Git (when not already done)
$ git init .

# 2. Add your App's Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Download a fortrabbit Craft .gitignore file
$ curl https://raw.githubusercontent.com/fortrabbit/craft-copy/master/src/.gitignore.example -o .gitignore
# This exclude the assets folder from Git, manage uploads outside via ssh/rsync/sftp

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'Initial commit'

# 6. Initial push and upstream
$ git push -u fortrabbit master
# The first push takes a little longer
# as it runs Composer

# 7. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting) and our [Git guide](/git).


## Next steps

Please also make yourself familiar with the options to deploy the Craft `assets` folder separately. There are two dedicated guides here, depending on your [App's Stack](/craft-3-about#toc-1-1-choose-your-stack): 

* Universal: [Deploy assets with rsync](/craft-3-assets-uni)
* Professional: [Deploy assets to the Object Storage](/craft-3-assets-pro).

Last not least, see our [Craft tuning guide](/craft-3-tune) to truly master Craft on fortrabbit.
