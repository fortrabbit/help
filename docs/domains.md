---

template:      article
reviewed:      2021-08-19
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
    - naked domain
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

Each fortrabbit [App](/app) has its own, unique [App URL](/app#toc-app-url). Additionally you can route any number of external domains to your App. Your goal here is to have your App running under your own domain.

First off, make sure that the App knows about the domain. Then point the domain to your fortrabbit App with your domain provider. Start the process in the fortrabbit Dashboard like so:

## Connect your domain to fortrabbit

1. Click Your App > Domains > "Add a new domain"
2. Choose a domain name
3. Use the provided informations to route the domain with your domain provider


## Basic setup

This is the most common usage. You first add the domain within the fortrabbit Dashboard. Use the preselected "www" option. That will bring you to a screen showing the current DNS settings and the desired settings. Go to your domain providers control panel and change the settings as provided by the fortrabbit Dashboard. That basically means:

For the www prefix: 

* Remove any A Record
* Point the CNAME Record to the fortrabbit App URL: {{ app-name }}.frb.io
* Leave NS or TXT or any other records untouched

For the naked domain:

* Change the A Record to use the IP of our forwarding service
* Leave any other records untouched, especially MX

Save the settings with your domain provider and wait a while, depending on your TTL settings that can take up to 24h. 


## Routing options

The world of DNS is one of its own. You want to do more than just the basics? Cool! Let's dive into it – understand the background, explore advanced and alternative settings.


### www and other subdomains

Back in the day the `www.` prefix indicated that this is an address to type into the browser. Nowadays the `www.` prefix indicates a "cloud-enabled" application which can be moved in seconds to another server location. The name of the subdomain prefix is not so important, but `www` is the convention for (marketing) entry points. For example: We use `help.fortrabbit.com` for the page you are currently reading and `blog.fortrabbit.com` to publish our thinkings.

The trick is that you can route subdomains using `CNAME` records. By this you are telling your DNS provider to resolve a `hostname` instead of a fixed `IP` (which would be an `A` record). The great advantage is that the IP address behind the hostname target can change later on — without your intervention. In other words: `CNAME` routing helps us to compensate hardware failures and DDOS attacks for you, since we can move your App around. 

### Naked domains

There are so called "naked", "APEX" or "root" domains. They have no prefix and look like so: `fortrabbit.com`. Some think that they are aesthetically more pleasing than their subdomain counterparts. But they don't play well as with cloud services — like ours. 

Naked domains should not be routed using a `CNAME` record; they should be routed using an `A`-Record. This is because of the specifications of DNS. You may want to have e-mails with your domain like `info@fortrabbit.com`, it is not possible to have with a CNAME record at the same time. Any domain routed to an IP is bound to that IP. This doesn't give us the flexibility to move your App around in case of incidents, for example with a DDoS attack.

Yes, naked domains may look more pleasing to the eye, but don't take this too seriously. All big players, like Google, use a `www.` subdomain, without you noticing, and most bigger sites you'll visit do the same. 

Safari and Chrome don't even show the `www.` prefix any more in the address bar. Firefox is greying out the protocol of this so-called trivial domain.

The `www.` prefix is so common, you'll hardly hardly recognize it. Is Facebook with `www.`? Is Google with `www.`? Is Wikipedia with `www.`? Yes, they all are and you don't care. It's just the best way to deal with DNS. 

We are providing a forwarding service, so that all requests on the naked domain will get forwarded to the `www.` domain, even deeplinks and https links. So you can still print the naked domain on flyers or in your e-mail signature.

Sometimes we have heard that the move from naked to `www.` can impact SEO in a negative way. This should not be the case, if you do it properly, as all your old URLs and deeplinks should be redirected to the new ones, using a standard `301 Moved Permanently` HTTP header. 

So please, as long as the naked domain works and will forward all requests, don't worry too much. If your boss still says so, use an external DNS provider that supports CNAME flattening (sometimes called ANAME): [here is help to do that on CloudFlare](/cloudflare#toc-using-cloudflare-for-naked-domains).


#### Don'ts

You could grab the IP of your App and use that as an `A`-record for your domain. It's technically possible, but then your App will be offline once we move it to a another Node (which changes the IP address).

In some cases you could just use `CNAME` routing for your naked domain. It's theoretically possible, but not recommended by the [DNS specs](http://www.ietf.org/rfc/rfc1035.txt), and would also break any e-mail delivery for your domain.


### Forwarding a naked domain to www

You should still care about your naked domain, as some users might type it in directly in the browser address bar. So you usually want to forward all requests from your naked domain to your primary canonical subdomain. That's why you'll get two routing values, the main `CNAME` target (for the `www.` domain) and an additional `A`-record (for the naked domain).

#### Example setup

```plain
HOSTNAME  TYPE       VALUE
--------------------------------------------
@         A-Record   52.18.136.112        < Only naked - IP depends on region, check Dashboard
www       CNAME      {{app-name}}.frb.io  < Only www!
```

#### Domain forwarding service explained

Optional but highly recommended: The fortrabbit domain forwarding service redirects all incoming requests on the naked domain to the `www.` version of the domain using `301 Moved Permanently` headers. It also works for deeplinks, so `http://your-domain.com/page` will be forwarded to `http://www.your-domain.com/page`. A custom SSL cert from Let's Encrypt for each naked domain will be issued, so that ALL communication can be secure (best combined with [.htaccess](/htaccess) rules on the `www.` side to force https). It needs to be set up as an `A`-record, you can not use `CNAME` on a naked domain, as that would possibly break your DNS settings (especially if you want to send e-mails). The service itself is independent from the App; it's the same IP for all Apps in one region.

#### Best practice for the redirect service

* Use `www.domain.tld` instead of just `domain.tld` for all links to avoid unnecessary 301 round-trips on each request. This includes internal links, advertisements and ping checkers.

#### Bad practices for the redirect service

In general you can expect our redirect service to work. But there might be services issues only affecting the redirect service. So better don't rely on it too much. Maybe even more important, requesting a resource from the redirect service means an additional http hop and that might make your website slower. Please, when using our redirect service, do yourself the favor:

* Don't configure your CMS to use the naked domain for all links (site_url)
* Don't send external links (advertisement clicks) through the naked domain
* Don't setup uptime checks for your naked domain
* Better setup the redirect within CloudFlare when using this

Please mind that all redirects on the redirect service are marked as 301 (moved permanently). That way search engines will pick up the www domain as the main domain anyways.


### Alternative ways to use a naked domain

You don't have to use our free domain forwarding service, here are some alternative options:

#### ALIAS / ANAME routing

In the fortrabbit Dashboard you can add a naked domain. To make this work you need a domain provider that supports so called "ALIAS" or "ANAME" records. They allow you to have the functionality of CNAME routing (hostname target instead of IP). This is a list of domain / DNS providers who offer these special records:

* **[DNSimple: ALIAS records](https://support.dnsimple.com/articles/alias-record/)**
* [DNS made easy: ANAME records](http://help.dnsmadeeasy.com/managed-dns/records/aname-records/)
* [Easy DNS: ANAME records](https://fusion.easydns.com/index.php?/Knowledgebase/Article/View/190/7/aname-records/)


#### Forwarding, using your domain provider

Some domain providers also support a simple HTTP redirect. Please see your domain provider's documentation. Here are some examples:

* [GoDaddy: domain forwarding](https://support.godaddy.com/help/article/422/manually-forwarding-or-masking-your-domain-name)
* [Gandi: domain forwarding](https://wiki.gandi.net/en/domains/management/domain-as-website/forwarding)
* [1&1: domain forwarding](http://help.1and1.com/domains-c36931/manage-domains-c79822/domain-destination-c38672redirectforward-your-domain-a594868.html)

#### Using a canonical tag

In the cases above you forward all requests to the ONE main domain you are using. In some cases you might have two domains serving the same content. Now, search engines need to know which page is the one they should show the results for. To help the search bots, you can use a canonical tag. 

Let's say you have the page `fortrabbit.com` and `fort-rabbit.com` registered with your fortrabbit App. And you want both to display the same content but still keep the originally entered URL. _Actually, for this example you might want to create a redirect to the domain that matters most to you._ Now you want the search engines to prefer and link to the first domain, so you add in the head of each HTML page delivered:

```
<head>
    …
    <link rel="canonical" href="https://www.fortrabbit.com">
</head>
```

More on [canonical URLs on Google webmasters](https://support.google.com/webmasters/answer/139066?hl=en)

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


- - -


## Choosing a domain provider

fortrabbit is not offering direct domain registration and management. In classical hosting, domain registration & e-mail hosting were bundled in packages. Today you can find specialized domain services. There are various offerings, so you should consider upfront what you'll need:

1. Additional IMAP/POP3 e-mail hosting
2. Additional SSL certificate ordering
3. Specialized in advanced DNS configurations
4. Exotic TLD-endings
4. General domain forwarding
5. Support for forwards with ALIAS or ANAME records

Many of our clients are using classical offerings combining domain ordering and e-mail hosting. Others are using separate services for domain registration and e-mail hosting.


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

Please see our [DNS troubleshooting guide](/).

