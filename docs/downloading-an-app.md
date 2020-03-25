---

template:      article
reviewed:      2020-03-24
title:         How to download a fortrabbit App
naviTitle:     Downloading an App
lead:          "This article helps you downloading all the data required directly from our services to run it elsewhere. This might help you: boarding a new developer, moving away to a different hosting provider, or just getting a fresh backup, or archiving your project before shutting it down. Tech skills are required to do this."
group:         tips
stack:         all

---


## Use cases

We highly advice to have your App in a local development environment running all the time. Your development setup should also be your master where development happens first and which is kept up-to-date in terms of contents. Please see [local development article](/local-development) for more.

But there are some real world scenarios why this still might be needed: 

* A new developer is joining the project
* A previous developer has left without transferring the project in a responsible manner
* Your harddisk crashed or you have accidentally deleted your local setup somehow




## Downloading the fortrabbit App data directly

Your App on fortrabbit consists of different parts that play together to display a functional website or application. The different parts need to be treated separately. The steps required are depending on your previous deployment workflows. Choose what matches best for you:

### Code via Git

When you deployed by Git here before, you can clone the existing Git repo from fortrabbit. Please follow the steps in our [Git guides](/git-deployment) to clone down the code. The benefit is that the Git repo contains all the history of the project as well.


### Get dependencies via Composer

After cloning the Git repo, you might need to install the dependencies to make your local installation complete. The `composer.json` contains all the instructions. Run `composer install` locally in your root folder. See our [Composer article](/composer) for more details.


### Runtime data excluded by Git




### MySQL





<!--  

### Craft Copy for Craft CMS

@Oli is it possible to use Craft Copy to clone down an App? So to start blank locally?

-->


## Using our backups

When the above is not working for you, you might just use our backups (when booked) as a source. Some plans on the Universal Stack have backups enabled. With that you can download the latest state as two files: code and MySQL database directly from the Dashboard. Please understand that this does not include the Git repo but just the files deployed and generated on the App itself. For Pro Apps backups are only optionally available for MySQL, use the 

## Checking data consistency

We highly advice to double-check your new backup for completeness. You can run diffs on other copies. We also advice to run your App locally and see it with your eyes.


## Contact us

If in doubt, don't hesitate to contact us. We are here to give technically guidance and help. Please mind fortrabbit is self service for developers. We might not be able to assist the non-technical business owner with this. 