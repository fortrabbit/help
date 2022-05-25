---

template:         article
reviewed:         2022-05-25
title:            Craft 3 setup
naviTitle:        Differences between Craft 3 and 4
lead:             In our help pages we cover the setup process of Craft 4. Here we document the differences to Craft 3. 
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
  - craftCMS4
  - craftCMS3
  - upgrade

---


## Use Craft 3 

Starting from May 25th 2022 we assume every new Craft App created on fortrabbit runs on Craft 4. To deploy a Craft 3 some changes are required.    

## Craft 3 Environment variables

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

The following ENV vars Craft 3 expects. Copy the block below and paste it under the ENV var settings of your App.
All you need to change is the `SECURITY_KEY`. You get it from your local .env file.


```osterei32
DB_DATABASE=${MYSQL_DATABASE}
DB_DRIVER=mysql
DB_PASSWORD=${MYSQL_PASSWORD}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
DB_DSN=mysql:host=${MYSQL_HOST};port=3306;dbname=${MYSQL_DATABASE}
ENVIRONMENT=production
SECURITY_KEY={REPLACE_WITH_YOUR_LOCAL_SECURITY_KEY}
```
