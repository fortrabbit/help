---

template:      article
reviewed:      2024-02-10 12:21:39
title:         Troubleshoot 504 errors
naviTitle:     504 errors
excerpt:       Some tips on troubleshooting 504 and 503 errors.
lead:          'The 504 HTTP status code means "Gateway timeout". The 503 HTTP status code means "Service Unavailable". Usually this means that the request is taking too long to process or something is blocking execution and many requests are piling up. This article aims to help developers troubleshooting 504 and 503 errors.'
group:         troubleshooting
stack:         all
dontList:      false
rank:          20
order:         20

---

## Typical symptoms of 504 errors

+ 504 errors will be shown after a longer time of trying to load a page
+ 504 errors here are sometimes rendered using a fortrabbit error page template
+ 504 errors often occur **without code changes**
+ 504 errors are sometime*s connected to traffic patterns
+ 504 errors are sometimes caused by editorial changes in a CMS
+ 504 errors often come and go - from offline to online and back again
+ 504 errors usually will not show up in the PHP error logs
+ Sometimes you'll find: "AH01079: failed to make connection to backend" in the logs, this is what Apache is reporting back

504 and [503 errors](/503-errors) are two sides of the same coin. PHP-FPM (the PHP process manager here) has too much to do. 504 and 503 errors are often happening simultaneously.

## Behavior

504 errors are usually a frustrating experience, they suddenly appear and are hard to trace.

### Errors out of the blue

Our experience shows that 504 (and 503) errors most often occur without direct interaction from the developer, like directly after deploying new code or changing configuration. 504 appear suddenly out of nowhere. In most cases this is not a server issue, but related to code and configuration.

### No traceable logs

504 and 503 errors are hard to debug, because usually no error logs are written, as the process, when it times out will not exist.

## Common causes

504 errors are usage based. The errors usually start to appear when a certain threshold is reached. Among many other reasons, this can be the number of concurrent visitors, the size of the database or certain actions by an editor in the control panel of a CMS. We have seen many 504 issues appearing without human interaction at all.

### Poorly performing database queries

The most common cause of individual downtime are slow database queries. A MySQL query might work in a testing environment, but not in production at a certain point of traffic and more data. Your application might be making MySQL database queries that are too heavy. MySQL queries can be CPU intensive and long running. Sometimes this is a technical requirement, often this is unawareness on the part of the developer.

Review your code for inefficient queries. If unsure consult a senior colleague or use a profiler like Blackfire to find the bottlenecks. Refactor your query design to write better performing MySQL queries or even rethink your data model. Caching certain database results can be another tactic after refactoring.

### Too many database queries

Your application might be making too many database queries. This issue is similar to the one above. Sometimes too many and poorly written MySQL queries come in tandem. In Craft CMS for example it's easy to write a lot of queries. Review your code and see if you can avoid or optimize queries.

### Image transforms

Image transformations can happen when an editor is uploading new photos into a CMS. For web delivery these images then need to be transformed into smaller sizes. imageMagick is often the tool of choice. Mind that such image crunching tasks are expensive in terms of performance - and they can stack up. Usually image transformations should only run once per image. Then the required sizes should be saved to the file system for later usage. This image cache should not be cleared in production.

For most use cases we advise lazily creating required images one by one instead of an eager (upfront) creation approach. The size of the original image also matters. One mitigation can be to use the GD library instead of imageMagick. Though it creates slightly more artifacts in JPG compression and maybe slightly bigger files, GD is often less resource hungry.

### External API calls

Sometimes a 504 error is caused when you call an external API that is not responding, or that responds too late. This should be relatively easy to debug. Try turning off any blocking external API calls.

### Bots

Sometimes bots come visiting your website, scraping each page to feed an AI or a search machine. When not configured correctly, your website may provide such bots an infinite of almost identical pages that still need to be rendered and cached.

### Plugins

CMS plugins are also a common source of performance problems. This includes plugins that are designed to improve performance. Sometimes plugins interact in unpredicted ways.

### Something else on application level

Basically anything that will end up in an endless loop can cause a 504 error. This can be a plugin of a CMS or something within your templates. Imagine a template that is endlessly including itself. It will go on for a while until either the memory, the CPU or the runtime come to an end.

There are many technical measurements which can be taken by the client to dramatically lower the chances of having any downtime at all. Our multi-node production plans are offering high availability when correctly implemented. For CMS systems such as Craft CMS, it's often possible to cache the whole frontend in a CDN by using edge caching with a third party service such as Cloudflare.

## Ways to proceed

- Decrease/increase the `max_timeout` setting for debugging
- Share results of your research with us in our client support

### Identifying 504 errors with the dashboard

The fortrabbit dashboard provides some useful metrics. You can see 5xx metrics by following the "Show all metrics" link. The 5xx metric is a mix of 500, 502, 503 and 504 errors. Have a look at the PHP response time metric as well. The PHP response time always goes up when there are 504 errors. Aim for no swap usage and a low PHP response time of not more than 200 ms.

### It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on our side, your hosting provider. Please check our status page under [status.fortrabbit.com](https://status.fortrabbit.com) if there are any ongoing maintenance windows or service issues known.
