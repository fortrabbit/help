---

template:      article
reviewed:      2021-02-12
title:         Troubleshoot 504 errors
naviTitle:     504 errors
excerpt:       Some tips on troubleshooting 504 errors.
lead:          'The 504 HTTP status code means "Gateway timeout". This means that the request is taking too long to process or something is blocking execution. This article aims to help developers troubleshooting 504 errors.'
group:         troubleshooting
stack:         all
dontList:      false

---

## 504 error behavior

* 504 errors will be shown after a longer time of trying to load a page
* 504 errors here are sometimes rendered using a fortrabbit error page template
* 504 errors often occur without code changes
* 504 errors are sometimes connected to traffic patterns
* 504 errors are sometimes caused by editorial changes in a CMS
* 504 errors often come and go – from offline to online and back again
* 504 errors usually will not show up in the PHP error logs
* Sometimes you'll find: "AH01079: failed to make connection to backend" in the logs, this is what  Apache is reporting back
* Sometimes even SSH and SFTP will not work for such ans App


## Common reasons and solutions

In most cases this is not a server issue, but something with your code and configuration.


### Poorly performing database queries

Your App might be making MySQL database queries that are too heavy. MySQL queries can be CPU intensive and long running. Sometimes this is a technical requirement, often this is unawareness on the part of the developer.

Review your code for inefficient queries. If unsure consult a senior colleague or use a profiler like [Blackfire](/blackfire) to find the bottlenecks. Refactor your query design to write better performing MySQL queries or even rethink your data model. Caching certain database results can be another tactic after refactoring.


### Too many database queries

Your App might be making too many database queries. This issue is similar to the one above. Sometimes too many and poorly written MySQL queries come in tandem. In Craft CMS for example it's easy to write a lot of queries.

Review your code and see if you can avoid or optimize queries.


### Image transforms with ImageMagick

Image transformations can happen when an editor is uploading new photos into a CMS. For web delivery these images then need to be transformed into smaller sizes. imageMagick is often the tool of choice. Mind that such image crunching tasks are expensive in terms of performance - and they can stack up.

Usually image transformations should only run once per image. Then the required sizes should be saved to the file system for later usage. This image cache should not be cleared in production.

For most use cases we advise lazily creating required images one by one instead of an eager (upfront) creation approach.

The size of the original image also matters.

One mitigation can be to use the GD library instead of imageMagick. Though it creates slightly more artifacts in JPG compression and maybe slightly bigger files, GD is way less resource hungry. You can change this with the PHP settings of your App in our Dashboard.


### External API calls

Sometimes a 504 error is caused when you call an external API that is not responding, or that responds too late. This should be relatively easy to debug. Try turning off any blocking external API calls.


### Something else on application level

Basically anything that will end up in an endless loop can cause a 504 error. This can be a plugin of a CMS or something within your templates. Imagine a template that is endlessly including itself. It will go on for a while until either the memory, the CPU or the runtime come to an end.


## Other ways to proceed

* Decrease/increase the `max_timeout` setting for debugging
* Try resetting the App from our Dashboard
* Share results of your research with us in our client support


## Identifying 504 errors with the Dashboard

The fortrabbit Dashboard provides some useful metrics. You can see 5xx metrics by following the "Show all metrics" link. The 5xx metric is a mix of 500, 502, 503 and 504 errors.

In order to separate 504 from the other 503, 502 and 500 errors, have a look at the PHP response time metric. The PHP response time always goes up when there are 504 errors. 

Aim for no swap usage and a low PHP response time of not more than 200 ms. 


## It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on our side, your hosting provider. Please check our status page under [status.fortrabbit.com](https://status.fortrabbit.com) if there are any ongoing maintenance windows or service issues known.


## Contacting support

Whether your investigation (see above) leads to possible service issues on our side or you are stuck debugging the issue on your own, don't hesitate to contact us.

Please include logs and additional information with your support request. 

* <a href="#asd" onclick="Intercom('showNewMessage', 'I see 504 for my App ______ for around ___. I have made the following changes recently: ____.')">Contact support</a>