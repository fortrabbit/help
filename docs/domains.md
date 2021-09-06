---

template:      article
reviewed:      2021-09-06
title:         All about domains & DNS
lead:          How to configure and route domains to your fortrabbit App.
naviTitle:     Domains
group:         platform
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

## No domain registration here

Please note that fortrabbit does not provide domain registration services. An "external domain" is a domain registered with a third party service. See [below](#toc-choosing-a-domain-provider) for more details.

## About domains on fortrabbit

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
* Point the CNAME Record to the fortrabbit App URL: {{ app-name }}.frb.io
* Leave NS or TXT or any other records untouched
* Remove any AAAA records

For the naked domain:

* Change the A Record to use the IP of our redirect service
* Leave any other records untouched, especially MX
* Remove any AAAA records

Save the settings with your domain provider and wait a while. The while depends on the TTL settings of the DNS records, or usually up to 24h.

## Routing options

The world of DNS is one of its own. You want to do more than just the basics? Cool! Let's dive into it - understand the background, explore advanced and alternative settings.

### www and other subdomains

Back in the day the `www.` prefix indicated that this is an address to type into the browser. Nowadays the `www.` prefix indicates a "cloud-enabled" application which can be moved in seconds to another server location. The name of the subdomain prefix is not so important, but `www` is the convention for (marketing) entry points. For example: We use `help.fortrabbit.com` for the page you are currently reading and `blog.fortrabbit.com` to publish our thinkings.

The trick is that you can route subdomains using `CNAME` records. By this you are telling your DNS provider to resolve a `hostname` instead of a fixed `IP` (which would be an `A` record). The great advantage is that the IP address behind the hostname target can change later on — without your intervention. In other words: `CNAME` routing helps us to compensate hardware failures and DDOS attacks for you, since we can move your App around.

### Bare domains

Please see the [dedicated article for bare domains and the redirect service](/bare-domains).

### Using CloudFlare

Please see our [CloudFlare article](/cloudflare) on how to setup and use CloudFlare together with fortrabbit.

### Wildcard domains

Do you want to route all requests for all possible subdomains to your fortrabbit App like so `[[*.your-domain.com]]`? That is possible.

#### Wildcard domain use cases

Some domain providers promote wildcard domains as the easy catch-all solution. With fortrabbit you should use a wildcard domain solely for the purpose when there is really any number of addresses that change dynamically. Imagine a social network App where your users will have `[[user]].[[your-domain.com]]` addresses.

When you actually only have a handful of sub-domains you need to cover, don't use a wildcard domain, register them manually with the Dashboard. A bonus is that those can be covered by the free Let's Encrypt certs. So when you plan for something like this: `www.domain.foo`, `help.domain.foo`, `blog.domain.foo`, `another.domain.foo` — don't register a wildcard domain. Wildcard domains are not an easy fix.

#### Wildcard domain verification

For security reasons we'll need to verify that you own the domain. You will need to verify an e-mail sent to this domain.

#### No free TLS for wildcard domains

We do not provide Let's Encrypt certificates for wildcard domains. So you need to bring your own certificate. See our [HTTPS article](/https) for more.

## Changing the default domain

Per default your App URL is the "default domain". You can change it so that links from the Dashboard and the thumbnail preview generation will use a custom domain of yours. We also use the default domain for various internal monitoring services.

To change the default domain: In the Dashboard go to your App > Domains. There click on the star icon beside the domain name in the list.

## Using HTTPS

Domains on fortrabbit can be accessed via `HTTP` and `HTTPS`. Please see the [HTTPS article](/https) for information on secured connections and SSL certificates.

## Setting a custom root path for a domain

A root path is a folder in the [file system](/directories) of the App where the domain will end. Per default all domains will have the same root path. The root of your App can be set under the settings of your App — See the [root path settings topic](/app#toc-root-path).

## Multiple domains on one App

There are some reasons why you might want to point more than one domain to your fortrabbit App:

### Host multiple websites in your App

You probably want to host more than one website on your App like so:

1. `your-first-domain.com` resolves to `htdocs/your-first-domain/public`
2. `your-other-domain.com` resolves to `htdocs/your-other-domain/public`
3. …

**Please don't do this!** It's the way VPS hosting works, but it's really not a good design pattern. Your App is NOT a server, there are good reasons to only host one project, website or application in one App. Please see our [App help article](/app#toc-one-website-per-app) to learn more.

### Forwarding other domains to your App

Let's say you have `your-domain.com` pointed to your App already. Now that other TLD, the domain `your-domain.co.uk` is  registered as well. So, of course, you want your App to show up under this domain as well. Now there are two main ways how this can be implemented:

1. `your-domain.co.uk/pricing` will forward to `your-domain.com/pricing`
2. `your-domain.co.uk/pricing` and `your-domain.com/pricing` will both show the same content

What you want, in most scenarios, is the first one: forwarding. Serving the same content under multiple domains is confusing — not only for humans but also for bots: The SEO spider bot might down-rank your content as a duplicate. You want a primary domain — one canonical name.

### Forwarding other domains with your domain provider

This should be preferred. You can forward one domain to another domain without any involvement of fortrabbit at all. Please check your domain provider to see if forwarding is allowed. The configuration should be dead simple. You point one domain to another and that's it.

* Do: forward using a HTTP `301 Moved Permanently` code.
* Don't: use a "masked forward" where the other domain is loaded into an iframe

Please keep an eye on HTTPS when forwarding. HTTPS will most likely not be provided by your domain provider. In most cases this is not issue.

### Forwarding other domains within your App

You might also do the forwarding programmatically within your App. The most common approach here is to work with `.htaccess` rules to redirect all requests to that other domain. You register each domain within the fortrabbit Dashboard and then catch all the requests in the App. Here is an example, which always redirects to a domain `www.some-domain.tld`:

```plain
RewriteCond %{HTTP_HOST} !^www\.some-domain\.tld [NC]
RewriteRule (.*) https://www.some-domain.tld/$1 [R=301,L]
```

This example is generic. Please check your framework or CMS for plug-ins or configurations.

### Configuring your domain for e-mail

So far we have covered how to route a domain to fortrabbit. To receive and send e-mails from your domain you will configure the MX record of your domain. Please see your e-mail hosting provider for instructions.

## Quirks

### Domain limit

Please mind that there is a limit of 100 domains per App. In many cases you can use a wildcard instead.

### No DNS settings for the App URL

When setting up an App we'll provide an App URL - see [App help](/app#toc-app-url). You can not configure any DNS entries for that App URL.

## Troubleshooting

Please see our [DNS troubleshooting guide](/dns-troubleshooting).

## Choosing a domain provider

fortrabbit is not offering direct domain registration and management. In classical hosting, domain registration & e-mail hosting were bundled in packages. Today you can find specialized domain services. There are various offerings, so you should consider upfront what you'll need:

1. Additional IMAP/POP3 e-mail hosting
2. Additional SSL certificate ordering
3. Specialized in advanced DNS configurations
4. Exotic TLD-endings
5. General domain forwarding
6. Support for forwards with ALIAS or ANAME records

Many of our clients are using classical offerings combining domain ordering and e-mail hosting. Others are using separate services for domain registration and e-mail hosting.
