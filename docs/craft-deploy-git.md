---

template:         article
reviewed:         2025-04-04 10:26:13
title:            Deploy Craft CMS with Git 
naviTitle:        Deploy Craft with Git
lead:             Learn how to deploy Craft CMS code base with Git to fortrabbit. 
group:            craft
stack:            all
oder:             10
dontList:         true

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

---



## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-start), and have [Craft installed locally](craft-install-local). This guide is for advanced users, making use of [Git](/git) and [Composer](/composer); it can be applied to [Professional Apps](/app-pro) and [Universal Apps](/app-uni) on fortrabbit. There is a more basic guide to install Craft using SFTP [over here](/craft-upload-sftp).


## Deploy the Craft code base with Git

Trigger the following commands in your **local** terminal:

```
# 1. Initialize Git (if not already done)
$ git init

# 2. Add your App's Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Add changes to Git
$ git add -A

# 4. Commit changes
$ git commit -m 'Initial commit'

# 5. Initial push and upstream
$ git push -u fortrabbit main
# The first push takes a little longer
# as it runs Composer

# 6. From there on only
$ git push
```

**Got an error?** Please see [access troubleshooting](/access-methods#toc-troubleshooting) and our [Git guide](/git).


## Next steps

Please also make yourself familiar with the options to deploy the Craft `assets` folder separately. There are two dedicated guides here, depending on your [App's Stack](/craft-start#toc-1-1-choose-your-stack): 

* Universal Apps: [Deploy assets with rsync](/craft-assets-uni)
* Professional Apps: [Deploy assets to the Object Storage](/craft-assets-pro).

Last not least, see our [Craft tuning guide](/craft-tune) to truly master Craft on fortrabbit.
