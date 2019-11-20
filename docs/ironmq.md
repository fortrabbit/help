---

template:         article
naviTitle:        IronMQ
reviewed:         2017-12-20
title:            Using IronMQ with fortrabbit
stack:            all

group:            Queues
section:          Extending_fortrabbit

deprecated:       yes
dontList:         true

websiteLink:      https://www.iron.io/platform/ironmq/
websiteLinkText:  iron.io/platform/ironmq
dataCenters:      n/a
image:            iron-mq-mark.png

keywords:
     - advanced

---


<!-- TODO: Delay until https://github.com/LaravelCollective/iron-queue uses IronMQ PHP SDK v4, which is required to use current IronMQ v3 -->



## About queues

 queue is a pipeline for long running tasks. You define jobs that line up in the queue and then will be executed one by one. Utilizing queues can substantially increase the user experience by reducing load times.


## About IronMQ

IronMQ is a popular elastic message queuing service. The service is provided by iron.io. It is highly available. It offers a HTTP/Rest-based API and monitoring.


## Pricing

It starts with a free plan which you can scale in amount of API requests, queue message size and some other factors. Their first paid plan starts at $29/month. You can see their pricing table only if you are logged in. Then it becomes available here: [hud.iron.io/account/plans](https://hud.iron.io/account/plans).


## Signing Up

Go to the [sign up page](https://hud.iron.io/users/new) and enter your name and email and choose a password. Wait for the confirmation mail, click on the link and you are in.


## Booking

Once you're logged in you are asked to create a new project. We recommend to name it `frbit-your-app` or the like.

Now you will see your project listed and a three buttons on the right side of the project name. Find the button right to your project name containing "MQ 3", which stands for "IronMQ version 3". Click it.

In the next screen you will be welcomed by the information that no queues have been created, yet. Before you create your first queue make sure that you do it in the right location. On the very top of the page, right of the "Iron.io" logo you'll find something like `mq-aws-eu-west-1-1`. That's the location chooser. Depending on where your fortrabbit App runs choose the right location:

* Europe: Choose `mq-aws-eu-west-1-*`
* USA: Choose `mq-aws-us-east-1-*`

Once that's decided you can click on the "Create Queue" button on the right side of the page. The queue name is up to you and you can created unlimited queues (even in the free plan). For starters we recommend something descriptive like `mails`. Keep `pull` as the queue type and finish with a click on "create". 

## Connecting

Go to the overview screen in the Iron.io control panel which shows you all your projects. Right under the project name you'll find a hexadecimal string which is your **project ID**. Note that down.

Now follow the Project > MQ 3 and find your "IronMQ API URL" (the **queue hostname**) on the right side. It looks like `mq-aws-eu-west-1-1.iron.io`. Note that down as well.

Lastly click on your account email in the upper right of the page and choose "My Tokens". Click the "*****" hiding your **account token** and note it down when it's shown, too.

Now a good idea is to take all three credentials and the queue name you chose before and store them in your App's [secrets](secrets-pro) by going to the fortrabbit Dashboard > Your App > Settings > App secrets and add:

```plain
IRON_MQ_HOST=your-queue-hostname
IRON_MQ_PROJECT_ID=your-project-id
IRON_MQ_TOKEN=your-account-token
IRON_MQ_QUEUE=your-queue-name
```

## Using IronMQ

IronMQ has great support in the PHP community. Some frameworks/CMS support it out of the box, for others there are libraries:

* [IronMQ SDK](https://github.com/iron-io/iron_mq_php)
* [Bernard, the queue library](https://github.com/bernardphp/bernard)

Also check out the [install guides](/#install-guides) and find out how to integrate IronMQ with your favorite framework or CMS.