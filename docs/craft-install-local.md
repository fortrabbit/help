---

template:         article
reviewed:         2023-11-15 15:53:02
title:            Install Craft CMS locally
naviTitle:        2. Install Craft locally
lead:             Learn how to install Craft CMS locally, matching your skills and workflows.
stack:            all
order:            2
group:            craft
rank:             80

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          4.0
supportLevel:     a

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

[Be ready](/get-ready), make sure to have a [local development environment](/local-development) up and running and be aware of the [different Craft deployment workflows](/craft-start) available. Use the detailed [official Craft 4 install guide](https://docs.craftcms.com/v4/installation.html) as your guideline to install Craft on your local machine first. You can safely skip this step when you already have an existing Craft 3 project running locally.


## Choose your Craft install workflow

The way you will install Craft will set the course on how you will [deploy](/deployment-methods) Craft CMS here on fortrabbit. **Choose your workflow now**:


### 1a. Download Craft with Composer

This is the recommended - more sophisticated - way. You will use [Git](/git) and [Composer](/composer#toc-local-composer) in the Terminal. Run this command **on you local machine** to create a Craft 4 project to get started:

```
$ composer create-project craftcms/craft {{app-name}}
```

See an error? Check your [local development](/local-development). Later on you can [deploy Craft with Git](/craft-deploy-git) either to Universal or Professional Apps on fortrabbit.


### 1b. Download the Craft zip file

Are you more "designer" and less "developer"? SFTP also works here. Just download Craft directly from the Craft website: [craftcms.com/latest-v3.zip](https://craftcms.com/latest-v3.zip). Unpack that zip file to get to the actual project files. Later on you can upload [Craft with SFTP](/craft-upload-sftp) on Universal Apps.


## Install Craft CMS locally

Craft 4 uses environment variables to access environment specific settings. With your fortrabbit Craft App, those variables are already set. Configure it to work on your local machine now. You have two options to install Craft:

### 2a. Terminal setup

```
# 1. go into your local Craft folder 
$ cd {{app-name}}

# 2. run the terminal installer
$ ./craft setup
```

This will ask you some questions, the defaults will work mostly, you can change these settings later.


### 2b. Browser setup

You can also run the installer in the browser by visiting this address: `http://{{host}}/index.php?p=admin/install` in your browser. Substitute `{{host}}` with the [host name of your local development environment](/local-development#toc-virtual-hosts). 

## Next steps

By now your Craft CMS should be running on your local development machine. You should be able to visit your Craft installation locally and log in to the Craft admin panel. Now it's time to [configure Craft](/craft-setup) to work in both environments.
