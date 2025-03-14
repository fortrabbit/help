---

template:      article
naviTitle:     "Universal Stack"
title:         "About the Universal Stack"
reviewed:      2023-11-15 15:52:17
lead:          'With each App you create, you can choose between two technology stacks. This article helps you to understand the Universal Stack.'
group:         stacks
stack:         uni
showAlways:    true

order:         2

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

Persistent storage is just a fancy word to describe regular storage, which won't be deleted. In contrary to [Ephemeral storage](app-pro#toc-ephemeral-storage), it allows you to use the website storage as you would with any VPS or shared hosting solution. All data your App writes is written to the disk and not removed upon deploy. In short: it is persistent.

## Logs

You can access either live logs or historic logs of your App. Please [read the logging article](logging-uni).


## Upgrading

The Universal plans differ in web storage size, MySQL size, available PHP memory and features like backups and cron jobs.

PHP's memory has an impact on the performance of your site. Too little PHP memory can also cause errors or even crashes on memory intensive tasks. 

The smallest plan is suited for hobby projects, landing pages, one-pagers, MVPs, weekend hacks, development, skeletons, personal blogs and whatever small project you can think of. The highest plan is suited for more serious intentions: you can put a commercial project there. Everything in between can be in between. [See the pricing page](https://www.fortrabbit.com/pricing) for specs.

If you need more horse power and options, please see how you can: [Migrate from Universal App to Professional App](/migrate-uni-to-pro).