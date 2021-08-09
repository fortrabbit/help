
---

template:      article
reviewed:      2021-08-07
title:         Downtime in practice
naviTitle:     Downtime tips
lead:          "Let's be realistic. Downtime will happen. Most issues are not caused by service interruption. There are different types and reasons of downtime. This page shows shows common misunderstandings and what clients can do to avoid downtime."
group:         tips
stack:         all

---


## Downtime caused by code and config issues

Most downtime we and our clients are experiencing is caused by "client coding mistakes" - usually by not making proper use of the available resources.

The most common cause of individual downtime are slow database queries. A MySQL query might work in a testing environment, but not in production at a certain point of traffic and more data. 

In most cases a "5xx server error" is not a hardware problem with the hosting but a runtime error in PHP caused by the specific code of the client. Such errors can be resolved independently by analyzing the logs files, which clients have access to. We are happy to discuss and research such problems together with our clients.

Also, there are many technical measurements which can be taken by the client to dramatically lower the chances of having any downtime at all. Our multi-node production plans are offering high availability when correctly implemented. For CMS systems such as Craft CMS, it's often possible to cache the whole frontend in a CDN by using edge caching with a third party service such as Cloudflare.

## Unclear states

Sometimes it's not easy to measure, track and see a downtime. There might be an issue making a website respond slower as usual, or something is an on/off state. If something can be considered down is sometimes related to the way this is measured, for example our monitoring systems might be reporting "200 OK", while a Pingdom alert from a client sends something else.

## Downtime without a clear cause



## Partial downtime

Often 