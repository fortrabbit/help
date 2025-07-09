---

template:      article
reviewed:      2025-07-09 14:40:51
title:         How to configure domains for fortrabbit
lead:          How to configure and route domains to your fortrabbit App.
naviTitle:     Basic domain configuration
order:         1
group:         dns
stack:         all

keywords:
    - TLD
    - Top Level Domain
    - top-level-domain
    - registration
    - ordering
    - zone apex
    - apex domain
    - root domain
    - subdomain
    - domain masking
    - domain name server
    - DNS
    - ns
    - lookup

---

## About domains on fortrabbit

Please note that fortrabbit does not provide domain registration services. An "external domain" is a domain registered with a third party service.

Each fortrabbit [App](/app) has its own, unique [App URL](/app#toc-app-url). Additionally you can "route" any number of external domains to your App. Essentially that means that you can host a PHP application under a dedicated domain name on fortrabbit, that you have registered with a third party domain name registrar.

First off, make sure that the App knows about the domain via the Dashboard. Then point the domain to the IP address of our redirect service (region-specific). Finally, set up a CNAME from www.domain.tld to the unique App URL provided by us.

Start the process in the fortrabbit Dashboard like so:

## Connect your domain to fortrabbit

1. Click Your App > Domains > "Add a new domain"
2. Choose a domain name
3. Use the provided information to route the domain with your domain provider

## Basic setup

This is the most common usage. You first add the domain within the fortrabbit Dashboard. Use the preselected "www" option. That will bring you to a screen showing the current DNS settings and the desired settings. Go to your domain providers control panel and change the settings as provided by the fortrabbit Dashboard. That basically means:

For the www prefix:

* Remove any A Record
* Point the CNAME Record to the fortrabbit App URL: {{app-name}}.frb.io
* Leave NS or TXT or any other records untouched
* Remove any AAAA records

For the naked domain:

* Change the A Record to use the IP of our redirect service
* Leave any other records untouched, especially MX
* Remove any AAAA records

Save the settings with your domain provider and wait a while. The while depends on the TTL settings of the DNS records, or usually up to 24h.
