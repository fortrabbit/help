---

template:         article
reviewed:         2025-09-15 08:19:56
title:            Using Blackfire with fortrabbit
naviTitle:        Blackfire
stack:            all

group:            Profiling
section:          Extending_fortrabbit

image:            blackfire-orig.png
websiteLink:      https://blackfire.io?utm_source=fortrabbit
websiteLinkText:  blackfire.io
dataCenters:      n/a


keywords:
    - profiler
    - performance
    - php
    - advanced
    - profiling
    - performance
    - testing

---


## About Blackfire

Blackfire empowers all developers and IT/Ops to continuously verify and improve their app's performance, throughout its life cycle - by getting the right information at the right moment. Blackfire enables to write performance tests that can be run along your standard test suite. It also provides recommendations to help you improve the performance of your application.

Using Blackfire in production will gain even better insights, once your App will receive traffic. Knowing about the performance of your code will make you a better developer and you will also likely be able to reduce hosting costs here by refactoring your code for a better performance. 


## Pricing

Blackfire is integrated with a BYO (Bring Your Own) licence model. You can use Blackfire free in your local development environment, which is already valuable. Additionally, as long as your App here is only running on your `*.frb.io` domain and you have no custom domain routed yet, you can use your free Blackfire plan on your fortrabbit App.

Once your App is ready for production and you have routed a domain, you will need a [Blackfire paid plan](https://blackfire.io/pricing). 


## Integration

To use Blackfire with your fortrabbit App, you only need to paste the Agent credentials from Blackfire into the fortrabbit [Dashboard](/dashboard). Here is a detailed step by step guide:


### Over at Blackfire

* Sign up or log in to [Blackfire](https://blackfire.io).
* Click your Profile > Account -> Credentials -> [My Server Credentials](https://blackfire.io/my/settings/credentials)
* Copy the credentials: Server ID and Server Token


### At fortrabbit

* Log in to the dashboard
* Navigate to your App > PHP > Debugging
* Enable Blackfire
* Paste the Blackfire Server ID and Server Token
* Save the PHP Settings
* Wait at least one minute, so that the containers are restarted with the new configuration


### At your browser

* Install the [Chrome Extension](https://blackfire.io/docs/integrations/chrome) or the [Firefox one](https://blackfire.io/docs/integrations/browsers/firefox) depending on your favorite browser
* Browse to your App
* Hit the Blackfire button to start profiling


## Limitations

There is no way to profile CLI calls (except through [PHP SDK](https://blackfire.io/docs/integrations/php/sdk)) for now.
