---

template:         article
naviTitle:        CloudAMQP
reviewed:         2019-09-207
title:            Using CloudAMQP with fortrabbit
group:            Queues
section:          Extending_fortrabbit
stack:            all

websiteLink:      https://www.cloudamqp.com/
websiteLinkText:  cloudamqp.com
category:         queues
dataCenters:      US, EU
company:          84codes AB
image:            cloudamqp-mark.png

keywords:
     - advanced

---



## About queues

A queue is a pipeline for long running tasks. You define jobs that line up in the queue and then will be executed one by one. Utilizing queues can substantially increase the user experience by reducing load times.


## About CloudAMQP

CloudAMQP is a hosted RabbitMQ service provided from "84codes AB" from Sweden. RabbitMQ implements the standardized [Advanced Message Queing Protocol (AMQP)](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol).


## Pricing

CloudAMQP starts with a free plan. You can scale by the amount of messages/month, concurrent connections and amount of queues. See the [CloudAMQP pricing page](https://www.cloudamqp.com/plans.html?utm_source=fortrabbit)


## Signing Up

You can sign up with just your e-mail (double-opt in) or your GitHub/Google account.


## Booking

Once you're logged click the "+ Create" button on the right side, which will lead you to the new instance screen. We recommend to use `frbit-your-app` as the Name. Depending on where your fortrabbit App runs choose the matching data center:

* fortrabbit App in EU: `Amazon Web Service` > `EU-West-1 (Ireland)`
* fortrabbit App in US: `Amazon Web Service` > `US-East-1 (Northern Virginia)`

Since you can scale later on at any point, we recommend to choose a small Plan, fitting with your needs. The plans page is linked from there if you are unsure how much you need.


## Connecting

In the instances list of the CloudAMQP console click on the "Details" button of your just created queue. Now open the fortrabbit Dashboard in another tab, navigate to Your App > Settings > App secrets and insert the CloudAMQP credentials as your App [secrets](/secrets):

```plain
# The "Server" from the CloudAMQP details
CLOUD_AMQP_HOST=moose.rmq.cloudamqp.com
CLOUD_AMQP_PORT=5672

# The "User & Vhost" from the CloudAMQP details
CLOUD_AMQP_USER=acbd123
CLOUD_AMQP_VHOST=acbd123

# The "Password" from the CloudAMQP details
CLOUD_AMQP_PASSWORD=acbd123
```

You can also use [environment variables](/env-vars) for the above. To use CloudAMQP from your fortrabbit App you need to do one more thing:


## Requesting a firewall white-listing

By default all outgoing calls from your fortrabbit App are blocked for security reasons. But you can request the fortrabbit team to open up any port for you. That doesn't take long and isn't complicated.

Login to the fortrabbit Dashboard, navigate to your App > Firewall whitelist and request a custom firewall rule. Write nothing under the optional IP field and insert the port `5672` in the Port field. As descriptions we suggest "CloudAMQP" or the like. Once your request has been approved, which usually takes not very long, you are ready to use your new queue!



## Using CloudAMQP

You can use any [AMQP library](https://packagist.org/search/?q=amqp). The most popular currently is [zircote/amqp](https://packagist.org/packages/zircote/amqp).

## Further reading

* [RabbitMQ official website](https://www.rabbitmq.com/)