---

template:    article
reviewed:    2019-09-28 
title:       How to move your App to fortrabbit
naviTitle:   Migrating to fortrabbit
lead:        How to transfer an application to fortrabbit.
group:       platform
stack:       all

---


WORD: fortrabbit is not your traditional hosting environment. It may require some tinkering on your application and we recommend reading about [the fortrabbit platform generals](app) as well as our [specific guides](/#install-guides) for various frameworks & CMS.

This article covers the general basics as well as some deep links for moving your App from any hosting provider to fortrabbit. This will hopefully cover everything you need to realize a smooth transition. Each App is different, adjust your plan accordingly and don't hesitate to [contact us](http://www.fortrabbit.com/contact) with your specific questions.





## Create your fortrabbit App

Each website or web application is represented by an [App](app) on fortrabbit. You can have as many Apps as you want. So in order to move your project you need to create an App on fortrabbit first. Do so in the fortrabbit [Dashboard](/dashboard).

### Choose a Stack

When creating an App, you'll be asked for a Stack. Small standard websites go into Universal, high performance web applications go into the Professional Stack. See our [stack article](/stacks) to learn more.


## Prepare your domains

To minimize downtimes caused by DNS cache propagation you should change the Time to Live (TTL) of your domains to the lowest possible value. Something like five minutes would be just great.

If you have some kind of web control panel with your domain provider, then you can most likely change it yourself. If not, just get in touch with the provider and ask them to reduce your TTL for you. You should do this 48-72 hours before you start with the migration.

After that's done make sure to add all the domains to your new fortrabbit App in the dashboard.


## Migrate your code

If you already subscribe to a Git based workflow, then there is probably not much to do here. If not, then you should familiarize yourself with Git. We promise: You won't regret it. Once you've used it, you won't go back without being forced.


## Migrate your runtime data

Runtime data means all kinds of data, which is created by your App at runtime. Usually these are user uploads or uploads from a CMS or some-such. When working with a Professional App or a clean Git workflow, the runtime data and the code should be separated.


## Migrate your databases

If your App is using a MySQL database, you will need to migrate the database data as well. [Export the MySQL database from your old hosting and import](mysql#toc-export-amp-import) it to the fortrabbit database.

## Sending e-mails

Simple `sendmail` won't work, see our [quirks article](/quirks#Mailing) on how to send mails, either via SMTP or 3rd party provider.

## HTTPS (optional)

All fortrabbit Apps can be accessed using a free HTTPS App URL (`https://{{app-name}}.frb.io`). Most Apps also offer free [HTTPS](/https) by Let's Encrypt for [custom domains](/domains).

## Final switch: DNS

Now that you have migrated your code, runtime data and database - and all the other stuff you needed - you are ready to push the button.

Now that your App is fully mirrored on fortrabbit and ready to handle traffic, you can [route your Domains DNS records to fortrabbit](domains#toc-route-a-custom-domain). If you waited the 48-72 hours for DNS caches to clear, downtime will be minimal as traffic is routed to your App on fortrabbit.

## Migrating away from fortrabbit

We don't lock you in. You can cancel at any time. Due to the distributed nature of services it's even easier to move somewhere else then. The steps are essentially the same as above.
