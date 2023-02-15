---

template:         article
reviewed:         2023-02-15 08:20:55
title:            Integrate New Relic with fortrabbit
naviTitle:        New Relic
lead:             Where should I improve the performance of my App? Learn how to combine the popular software analysis tool with fortrabbit.
group:            Profiling
section:          Extending_fortrabbit
stack:            all

image:            new-relic-logo.svg
websiteLink:      https://www.newrelic.com?utm_source=fortrabbit
websiteLinkText:  newrelic.com
dataCenters:      n/a

---



## About New Relic

[New Relic](https://www.newrelic.com) is a suite of professional software analysis tools. It offers analytics and deep insights on your Apps' performance and possible bottle necks. New Relic integrates as a PHP extension, which is installed (but not enabled) on all fortrabbit Apps.


## Usage

fortrabbit has a Bring-Your-Own-License integration with New Relic. That basically means: you need an account over at New Relic. Then you only need to paste the license key in our dashboard.

### Over at New Relic

1. Sign up or log in to [New Relic](http://newrelic.com/).
1. Add a new application
1. Copy the license key

### At fortrabbit

1. Log in to the Dashboard
1. Navigate to your App > Settings > PHP > Debugging
1. Paste the New Relic license key
1. Save the PHP Settings

You are all good! After 5 minutes you should start to see data in your NewRelic dashboard. To get more information about how to use NewRelic, head to their [documentation](https://docs.newrelic.com/docs/apm/new-relic-apm/getting-started/introduction-new-relic-apm).