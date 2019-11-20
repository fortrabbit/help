---

template:         article
naviTitle:        mLab
reviewed:         2019-09-30
title:            Using mLab with fortrabbit
group:            Databases
section:          Extending_fortrabbit
stack:            all

websiteLink:      https://mlab.com?utm_source=fortrabbit
websiteLinkText:  mlab.com
category:         databases
dataCenters:      US, EU
image:            mlab-mark.png

keywords:
     - advanced
     - mongodb
     - DBaaS
     - ORM

---


## About MongoDB

MongoDB is a widely used, open-source NoSQL database. It stores data using a flexible document data model — similar to JSON.


## About mLab

mLab, formally known as MongoLab, is a MongoDB-as-a-Service provider. Many fortrabbit clients are using it.


## Pricing

It starts with a free plan with an fair amount of storage included. From there you can continue on shared or dedicated instances.


## Signing Up

Go to the [mLab sign up page](https://mlab.com/signup?utm_source=fortrabbit) and enter your credentials to sign up for an account.


## Booking

Once you are logged in click on the "Create new" button. In the next dialog, choose "amazon web services" as the "cloud provider". In the below "Location" chooser use the location fitting with the placement of your fortrabbit App:

* Europe: Choose `Amazon's EU (Ireland) Region (us-west-1)`
* USA: Choose `Amazon's US East (Virginia) Region (us-east-1)`

Now you need to decide which plan to choose. First mind that you can later on upgrade, so no need to panic if you do not exactly know what you are doing. As a guideline: If you are using fortrabbit redundant Production level plans, then you want to use mLab's "Replica set cluster", because they are redundant as well. If you just want to test things out, then choose the "Single-node Sandbox" plan, which is free.

Before you finish the booking you must decide your database plan. We recommend something like `frbit-your-app`. Then you can click on "Create new MongoDB deployment" and wait for the deployment to process.

Once your database deployment has been created, you will find yourself in the list overview. Click on your database name, which looks something like "ds123456/frbit-your-app". This will lead you to the database detail dialog, in which you now need to create a database user. Click on the "Users" tab and then "+ Add database user". The user credentials are up to you, just make sure to note them down for later. We recommend for user name something like `frbit-your-app-usr1`.

## Connecting

If you are not still there, go to the just created database detail view. Here you will find on the top of the view the connecting instructions. Among those is the shell connection command, which looks something like this:

```plain
% mongo ds123456.mlab.com:23456/frbit-your-app -u <dbuser> -p <dbpassword>
```

What you need to take from this are two things: Your **hostname**, which would be `ds123456.mlab.com` in this case and your **port**, which would be `23456` in this case.

We recommend to stash those information and the user credentials from before in your App's [secrets](secrets). Go to the fortrabbit Dashboard > Your App > Settings > App Secrets and add:

```plain
# the hostname and port you got
MLAB_HOST=ds123456.mlab.com
MLAB_PORT=23456

# the user name and password you chose
MLAB_USER=frbit-your-app-usr1
MLAB_PASSWORD=your-password
```

### 1. Request a firewall white-listing

By default all outgoing calls to non-standard ports from your fortrabbit App are blocked for [security](security) reasons. But you can request the fortrabbit team to open up any port for you. That doesn't take long and isn't complicated.

Navigate in the fortrabbit [Dashboard](dashboard) to your App > Settings > Firewall whitelist and request a new custom firewall rule. Write nothing under the optional IP field and insert the port you noted down before in the Port field. As descriptions we suggest "MongoDB on mLab" or the like. Once your request has been approved, which usually takes not very long, you are ready to use your new MongoDB database!

### 2. Enable the PHP extension

While you are logged in the Dashboard, navigate to your App > Settings > PHP Settings and enable the `mongodb` extension - or the `mongo` extension, if you are still using PHP 5.6.

## Using mLab

MongoDB is supported by many PHP frameworks and CMS out of the box. As of now, the most popular composer package are:

* [doctrine/mongodb](https://packagist.org/packages/doctrine/mongodb) (works only with PHP 5.6)
* [mongodb/mongodb](https://packagist.org/packages/mongodb/mongodb)

For specific integrations check out the [install guides](/#install-guides) and find out how to use it with your favorite framework or CMS.
