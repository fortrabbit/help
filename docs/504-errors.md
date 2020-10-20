---

template:      article
reviewed:      2020-10-19
title:         504 errors
naviTitle:     504 errors
excerpt:       What you may want to know about our 404 errors.
lead:          'The 504 HTTP status code is "Gateway timeout". This means that the request is taking too long to process or something is blocking execution.'
group:         troubleshooting
stack:         all
dontList:      false

---


Usually when you are trying to call the page initially, nothing will happen and it keeps loading until it will finally show the 504 page. 


## Need to know

* 504 errors are mostly connected to code you have written or you are using
* 504 errors often occur out of the blue, without changes from your side
* 504 errors are sometimes connected to traffic patterns
* 504 errors are sometimes caused by changes done within a CMS by editors
* 504 errors often come and go, from offline to online and back again
* It’s not likely that you can find something in the PHP error logs


## Common causes of 504 errors

* too many database queries
* poorly performing database queries 
* image transforms (ImageMagick) 
* external API calls
* something else on application level (CMS plugins for example) 


## How to proceed

* What is the request process with your page?
* Are you doing image transformations? How? Can you cache them?
* Are you making calls to external APIs?
* Is your page generating an expensive database query?
* Is there maybe some sort of loop you are creating in your code?
* Use a profiler like [Blackfire](/blackfire) or [NewRelic](/new-relic) to find the bottleneck!
* You can also decrease/increase max_timeout setting for debugging
* Try resetting the App from our Dashboard
* Share results of your research with us


## Identifying 504 errors with the Dashboard

The fortrabbit Dashboard is providing some useful metrics. You can see 5xx metrics by following the "Show all metrics" link with your App. The 5xx metric is a mix of 500, 502, 503 and 504 errors.

In order to separate 504 from the other 503, 502 and 500 errors, have a look at the PHP response time metric. The PHP response time always goes up, when there are 504 errors. In return not always when the PHP response goes up, 504 errors occur.
