---

template:      article
naviTitle:     "Universal Stack"
title:         "About the Universal Stack"
reviewed:      2019-09-30
lead:          'With each App you create, you can choose between two technology stacks. This article helps you to understand the Universal Stack.'
group:         stacks
stack:         uni
showAlways:    true

---


The Universal Stack is made for general purpose PHP web development — websites and web applications. Its design unifies legacy development workflows with modern web development paradigms.

## Target audience

The Universal Stack is, well, universally usable. Everyone should be able to use the Universal stack out-of-the-box. It's a good start for developers new to the cloud to get their feet wet without having to learn a bunch of new technologies and workflows.


## Specs, limits, purpose

* [Visit our specs page](https://www.fortrabbit.com/specs) to see some hard numbers
* [See our stack article](/stacks) to learn more about the options


## Application types

Unless your project is very resource intensive, there is virtually no application type which won't fit. Depending on the chosen plan, coding skills, data volume and traffic you can use Universal Stack Apps for anything you like, like: semi-static websites, small e-shops, all kind of blogs, weekend projects, development, staging, MVPs to CMS driven websites and small web applications.


## Persistent storage

Persistent storage is a fancy word to describe: it's just regular storage, which won't be deleted. In contrary to [Ephemeral storage](app-pro#toc-ephemeral-storage), it allows you to use the website storage as you would with any VPS or shared hosting solution. All data your App writes is written to the disk and not removed upon deploy. In short: it is persistent.

## Logs

You can access either live logs or historic logs of your App. Please [read the logging article](logging-uni).


## Upgrading

The Universal plans differ in web storage size, MySQL size, available PHP memory and features like backups and cron jobs.

The PHP memory has an impact on the performance of your site. Too little PHP memory can also cause errors or even crashes on memory intensive tasks. 

The smallest plan is suited for hobby projects, landing pages, one-pagers, MVPs, weekend hacks, development, skeletons, personal blogs and whatever small project you can think of. The highest plan is suited for more serious intentions: you can put a commercial project there. Everything in between can be in between. [See the pricing page](https://www.fortrabbit.com/pricing) for specs.

If you need more horse power and options, please see how you can: [Migrate from Universal App to Professional App](/migrate-uni-to-pro).

## Downgrading

Sorry, at this time it is not possible to downgrade from a higher to a lower plan in the Universal Stack. While this would be nice for users - please believe that it is not our aim to lock you in - it would be quite complicated to achieve. To downgrade, we would have to implement several limit checks and give infos in the Dashboard which limits should have be to adjusted before downgrading.

As an alternative we suggest to throw away the current App and create a new one: download the [backups](/backups-uni), delete the App, create a new App with correct plan and upload (or deploy) again.

Thanks for understanding!

<!-- apache-util2 not installed, will not work that way.

## Password authentication with http-auth

```shell
# 1. login to your App via SSH


# 2. create a .htpasswd file containing a "username" in the tmp folder
htpasswd -c /srv/tmp/.htpasswd username
# You'll be asked for a password

# 3. 

```
-->