---

template:      article
reviewed:      2023-02-15 08:20:18
title:         DNS and domain troubleshooting
naviTitle:     DNS troubleshooting
excerpt:       Troubleshooting DNS issues
lead:          'DNS is hard. This article aims to help you - the developer - solving common problems when connecting a domain to your fortrabbit App.'
group:         dns
stack:         all
dontList:      false
order:         10

---


## Using the fortrabbit Dashboard as a guidance

When visiting your domain within the Dashboard, you'll see two tables: On the left you see the desired settings of the domain. On the right, you'll see the current, actual settings.

In general — when not using an external DNS service like Cloudflare — those two settings should match. The left and the right side should be the same.

## Wrong DNS settings for the naked domain

Issues setting up the redirect for the naked domain are not uncommon. Please bear in mind the difference between the naked domain and the `www.` domain. You have one domain registered with your provider, but from a DNS point of view, those things are different:

1. `www.mydomain.com` < the www-domain
2. `mydomain.com` < the naked domain

The DNS settings described here need to be applied in the right place:

* The IP address for the A-Record is for the naked domain ONLY.
* The CNAME is for the `www.` domain ONLY.

When registering a `www.` domain with fortrabbit we'll provide an IP address for an A-Record. This one is for the naked domain only. This IP will forward all requests to that domain to the `www.` version.

So, in general: If your `www.` domain has an A-Record, there might be something wrong. If your naked domain has a CNAME record, there is definitely something wrong.

Please check the domain in the Dashboard > {{app-name}} > domains > {{domain-name}}. That view shows you two tables: The one on the left is the desired setup and shows two rows: one for the naked, one for the `www.` domain. On the right hand side you see the same table, but this one shows you the actual current settings. If these match, everything will work.

## No DNS settings

Depending on your browser you might see a _DNS_PROBE_FINISHED_NXDOMAIN_ or _This site can't be reached_ or _This webpage is not available_ error. In such cases, it is likely that the domain is not registered or there are no DNS settings for that domain. You can verify that with the [dig command](/dig). You need to fix that with your domain provider.

## Time delays

Many DNS providers default the TTL (Time To Live) of all records to 24 hours. This means that all changes will be applied with a delay of up to 24 hours, because everybody who has looked up the domain caches the result for the TTL duration. Also the TTL is not guaranteed: One badly programmed router on the way, who ignores the TTL or imposes its own minimal TTL, can change the TTL for everybody "behind it". The caching itself is actually a good thing as it helps to reduce round trips. Some providers let you set down the TTL, which is very useful when moving or changing domains.
