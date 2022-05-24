---

template:         article
reviewed:         2022-05-25
title:            Craft 3 setup
naviTitle:        Differences between Craft 3 and Craft 4
lead:             In our help pages we cover the setup process of Craft 4. Here we document the differences to Craft 3. 
group:            craft
stack:            all
dontList:         false

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

Starting from May 27th 2022 we assume every new Craft App created on fortrabbit runs on Craft 4. To deploy a Craft 3 some changes are required.    

## Craft 3 Environment variables


```
DB_DATABASE=${MYSQL_DATABASE}
DB_DRIVER=mysql
DB_PASSWORD=${MYSQL_PASSWORD}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
DB_DSN=mysql:host=${MYSQL_HOST};port=3306;dbname=${MYSQL_DATABASE}
ENVIRONMENT=production
SECURITY_KEY={REPLACE_WITH_YOUR_LOCAL_SECURITY_KEY}
```
