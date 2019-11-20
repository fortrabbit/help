---

template:      article
reviewed:      2019-09-16
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

The world of DNS is one of its own. You want to some more than just the basics? Cool! Let's dive into it – understand the backgrounds, explore advanced and alternative settings.


### www and other subdomains

Back in the days the `www.` prefix indicated that this is an address to type into the browser. Nowadays the `www.` prefix indicates a "cloud-enabled" application which can be moved in seconds to another server location. The name of the subdomain prefix is not so important, but `www` is the convention for (marketing) entry points. For example: We use `help.fortrabbit.com` for the page you are currently reading and `blog.fortrabbit.com` to publish our thinkings.

The trick is that you can route subdomains using `CNAME` records. By this you are telling your DNS provider to resolve a `hostname` instead of a fixed `IP` (which would be an `A` record). The great advantage is that the IP address behind the hostname target can change later on — without your intervention. In other words: `CNAME` routing helps us to compensate hardware failures and DDOS attacks for you, we can move your App around. 

### Naked domains

There are so called "naked", "APEX" or "root" domains. They have no prefix and look like so: `fortrabbit.com`. Some think that they are aesthetically more pleasing than their subdomain counterparts. But they don't play well as with cloud services — like ours. Naked domains should not be routed using a `CNAME` record; they should be routed using an `A`-Record. An domain routed to a an IP is static. It doesn't give us the flexibility to move your App around.

Yes, naked domains may look more pleasing to the eye, but don't take this too serious. Look, all big players, like Google are using a www. subdomain, without that you ever noticed, most bigger sites you'll visit do as well. 

Safari and Chrome don't even show the `www.` suffix any more in the address bar. Firefox is greying out the protocol of this so called trivial domain.

The www prefix is so common, you'll hardly hardly recognize it. Is Facebook with www? Is Google with www? Is Wikipedia with www? Do you know? Yes, they all are and you don't care. It's just the best way to deal with DNS. 

We are providing a forwarding service, so that all requests on the naked domain will get forwarded to the www domain, even deeplinks and https links. So you can still print the naked domain on flyers or in your e-mail signature.

Sometimes we have heard that the move from naked to www can impact SEO in a negative way. This should not be the case, when you do it properly, as all your old URLs and deeplinks shall be redirected to the new ones, using a standard 301 moved permanently HTTP header. 

So please, as long as the naked domain works and will forward all requests, don't bother too much. If your boss still says so, use an external DNS provider that support CNAME flattening (sometimes called ANAME), [here is help to do that on CloudFlare](/cloudflare#toc-using-cloudflare-for-naked-domains).


#### Don't dos

You could grab the IP of your App and use that as an `A`-record for your domain. It's technical possible, but than your App will be offline, once we move it to a another Node (which changes the IP address).

In some cases you could just use `CNAME` routing for your naked domain. It's theoretically possible, but not recommended by the [DNS specs](http://www.ietf.org/rfc/rfc1035.txt) and also would break any e-mail delivery for your domain.


### Forwarding a naked domain to www

You still should care about your naked domain, as some users might type it in directly in the browser address bar. So you usually want to forward all requests from your naked domain to your primary canonical subdomain. That's why you'll get two routing values, the main `CNAME` target (for the www domain) and an additional `A`-record (for the naked domain).

#### Example setup

```plain
HOSTNAME  TYPE       VALUE
--------------------------------------------
@         A-Record   52.18.136.112        < Only naked - IP depends on region, check Dashboard
www       CNAME      {{app-name}}.frb.io  < Only www!
```

#### Domain forwarding service explained

Optional but highly recommended: The fortrabbit domain forwarding service redirects all incoming requests on the naked domain to the `www.` version of the domain using `301 Moved Permanently` headers. It also works for deeplinks, so `http://your-domain.com/page` will be forwarded to `http://www.your-domain.com/page`. A custom SSL cert from Let's Encrypt for each naked domain will be issued, so that ALL communication can be secure (best combine with [.htaccess](/htaccess) rules on the www side to force https). It needs to be set up as an `A`-record, you can not use `CNAME` on a naked domain, as that would possibly break your DNS settings (especially if you want to send e-mails). The service itself is independent from the App it's the same IP for all Apps in one region.


### Alternative ways to use a naked domain

You don't have to use our free domain forwarding service, here are some alternative options:

#### ALIAS / ANAME routing

In the fortrabbit Dashboard you can add a naked domain. To make this work you need a domain provider that supports so called "ALIAS" or "ANAME" records. They allow you to have the functionality of CNAME routing (hostname target instead of IP). Following a list of domain / DNS providers who offer these special records:

* **[DNSimple: ALIAS records](https://support.dnsimple.com/articles/alias-record/)**
* [DNS made easy: ANAME records](http://help.dnsmadeeasy.com/managed-dns/records/aname-records/)
* [Easy DNS: ANAME records](https://fusion.easydns.com/index.php?/Knowledgebase/Article/View/190/7/aname-records/)


#### Forwarding, using your domain provider

Some domain providers also support a simple HTTP redirect. Please see your domain providers documentation. Here are some examples:

* [GoDaddy: domain forwarding](https://support.godaddy.com/help/article/422/manually-forwarding-or-masking-your-domain-name)
* [Gandi: domain forwarding](https://wiki.gandi.net/en/domains/management/domain-as-website/forwarding)
* [1&1: domain forwarding](http://help.1and1.com/domains-c36931/manage-domains-c79822/domain-destination-c38672redirectforward-your-domain-a594868.html)

#### Using a canonical tag

In the cases above you have forwarded all requests to the ONE main domain you are using. In some cases you might have two domains serving the same content. Now, search engines need to know which page is the one they should show the results for. To hint the search bots, you can use a canonical tag. 

Let's say you have the page `fortrabbit.com` and `fort-rabbit.com` registered with your fortrabbit App. And you want both to display the same content but still keep the originally entered URL. _Actually, for this example you might want to create a redirect to the domain that matters most to you, anyways, it's just an example._ Now you want the search engines to prefer and link to the first domain, so you add in the head of each HTML page delivered:

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

When you actually only have a handful of sub-domains you need to cover, don't use a wildcard domain, register them manually with the Dashboard. A bonus is that those can be covered by the free Let's Encrypt certs. So when you plan for something like this: `www.domain.foo`, `help.domain.foo`, `blog.domain.foo`, `another.domain.foo` — don't register a wildcard domain. Wildcard domains are not the easy fix.

#### Wildcard domain verification

For security reasons we'll need to verify that you own the domain. You will need to verify an e-mail send to this domain.

#### No free TLS for wildcard domains

We do not provide Let's Encrypt certificates for wildcard domains. So you need to bring your own certificate. See our [HTTPS article](/https) for more.


## Changing the default domain

Per default your App URL is the "default domain". You can change it so that links from the Dashboard and the thumbnail preview generation will uses a custom domain of yours. We also use the default domain for various internal monitoring services.

To change the default domain: In the Dashboard go to your App > Domains. There click on the star icon beside the domain name in the list.

## Using HTTPS

Domains on fortrabbit can be accessed via `HTTP` and `HTTPS`. Please see the [HTTPS article](/https) for informations on secured connections and SSL certificates.


## Setting a custom root path for a domain

A root path is a folder in the [file system](/directories) of the App where the domain will end. Per default all domains will have the same root path. The of your App can be set under the settings of your App — See the [root path settings topic](/app#toc-root-path).


- - -


## Choosing a domain provider

fortrabbit is not offering direct domain registration and management. In classical hosting, domain registration & e-mail hosting where bundled in packages. Today you can find specialized domain services. There are various offerings, so you should consider upfront what you'll need:

1. Additional IMAP/POP3 e-mail hosting
2. Additional SSL certificate ordering
3. Specialized in advanced DNS configurations
4. Exotic TLD-endings
4. General domain forwarding
5. Support for forwards with ALIAS or ANAME records

Many of our clients are using classical offerings combining domain ordering and e-mail hosting. Others are using separated services for domain registration and e-mail hosting.


## Multiple domains on one App

There some reasons why to point more than one domain to your fortrabbit App:

### Host multiple websites in your App

You probably want to host more than one website on your App like so:

1. `your-first-domain.com` resolves in `htdocs/your-first-domain/public`
2. `your-other-domain.com` resolves in `htdocs/your-other-domain/public`
3. …

**Please don't do this!** It's the way VPS hosting works, but it's really not a good design pattern. Your App is NOT a server, there are good reasons to only host one project, website or application in one App. Please see our [App help article](/app#toc-one-website-per-app) to learn more.

### Forwarding other domains to your App

Let's say you have `your-domain.com` pointed to your App already. Now that other TLD, the domain `your-domain.co.uk` is  registered as well. So, of course, you want your App to show up under this domain as well. Now there are two main ways how this can be implemented:

1. `your-domain.co.uk/pricing` will forward to `your-domain.com/pricing`
2. `your-domain.co.uk/pricing` and `your-domain.co.uk/pricing` will both show the same content

What you want, in most scenarios, is the first one: forwarding. Serving the same content under multiple domains is confusing — not only for humans bur also for bots: The SEO spider bot might down-rank your content as a duplicate. You want a primary domain — one canonical name.

### Forwarding other domains with your domain provider

This should be preferred. You can forward one domain to another domain without any involvement of fortrabbit at all. Please check your domain provider if forwarding is allowed. The configuration should be dead simple. You point one domain to another and that's it.

* Do: forward using a HTTP 301 code (moved permanently).
* Don't: a "masked forward" where the other domain is loaded into an iframe

Please keep an eye on HTTPS when forwarding. HTTPS will most likely not be provided by your domain provider. In most cases this is not issue.


### Forwarding other domains within your App

You might also do the forwarding programmatically within your App. The most common approach here is to work with `.htaccess` rules to redirect all requests to that other domain. You register each domain within the fortrabbit Dashboard and then catch all the requests in the App. Following an example, which always redirects to a domain `www.some-domain.tld`:

```plain
RewriteCond %{HTTP_HOST} !^www\.some-domain\.tld [NC]
RewriteRule (.*) https://www.some-domain.tld/$1 [R=301,L]
```

This example is generic. Please check your framework or CMS for plug-ins or configurations.


- - -

## Troubleshooting DNS

DNS is hard. When visiting your domain within the Dashboard, you'll see two tables: On the left you see the desired settings of the domain. On the right, you'll see the current, actual settings. In general — when not using an external DNS service like Cloudflare — those two settings should match. The left and the right side should be the same.

###  Wrong DNS settings

Please mind the difference between the naked and the www domain. You have one domain registered with your provider, but from a DNS point of view, those things are different:

1. `www.mydomain.com` < the www-domain
2. `mydomain.com` < the naked domain

The DNS settings described here need to be applied on the right place:

* The IP address for the A-Record is for the naked domain ONLY. 
* The CNAME is for the www-domain ONLY. 

When registering a `www.` domain with fortrabbit we'll provide an IP address for an A-Record. This one is for the naked domain only. This IP will forward all requests to that domain to the www version.

So, in general: When your www. domain has an A-Record, there might be something wrong. So when your naked domain has a CNAME record, there for sure is something wrong.

Please check the domain in the Dashboard > {{app-name}} > domains > {{domain-name}}. That view shows you two tables: The one on the left is the desired setup and shows two rows: one for the naked, one for the www-domain. On the right hand side you see the same table, but this one shows you the actual current settings. If those match, everything will work.

Please see the [example setup above](#toc-example-setup). 



### Dig

You can use the terminal to see the current DNS settings of your domain. With the `dig` command you can see if there are any CNAME entries and where they are pointing to. Here we lookup `help.fortrabbit.com` and see a CNAME pointing to the App URL: `help-frbit.frb.io`.

```
$ dig help.fortrabbit.com
;; Truncated, retrying in TCP mode.

; <<>> DiG 9.8.3-P1 <<>> ANY help.fortrabbit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44565
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;help.fortrabbit.com.       IN  ANY

;; ANSWER SECTION:
help.fortrabbit.com.      600    IN  CNAME   help-frbit.frb.io.
help-frbit.frb.io.        300    IN  CNAME   help-frbit.eu2.frbit.net.
help-frbit.eu2.frbit.net.  20    IN  A       52.48.51.144

;; Query time: 863 msec
;; SERVER: 192.168.178.1#53(192.168.178.1)
;; WHEN: Wed Jun  1 10:25:40 2016
;; MSG SIZE  rcvd: 122
```

Alternately you can use a browser based DNS lookup tool. See [these results](http://lmgtfy.com/?q=dns+lookup).

#### Dig an IP

```bash
# This will print out the IP of your App
$ dig +short {{app-name}}.frb.io 
```

See also [here](/quirks#toc-outgoing-ip) why you'll probably need your Apps IP.

### Time delays

Many DNS providers default the TTL (Time To Live) of all records to 24 hours. This means that all changes will apply with a delay up to 24 hours, because everybody who has looked up the domain caches the result for the TTL duration. Also the TTL is not guaranteed: One badly programmed router on the way, who ignores the TTL or imposes it's own minimal TTL, can change the TTL for everybody "behind it". The caching itself is actually a good thing as it helps to reduce round trips. Some providers let you set down the TTL, which is very useful when moving or changing domains.


### Testing domain routing with your local hosts file

Let's say you are developing a new    App and want to use your custom domain before actually routing it to fortrabbit. Just add the domain to fortrabbit, as you would do with any actually routed domain, then modify your local host file, which let's your local machine now ahead of time that the domain is to be served from your fortrabbit App.

#### hosts file location

The hosts file is a text file (without file type ending). It can be found here:

* Windows: `c:\windows\system32\drivers\etc\hosts`
* MacOS & Linux: `/etc/hosts`

#### How to get your Apps IP

See the above troubleshooting section to grab the current IP of your App. Mind that this IP will change in the future and that this IP is just for now.

#### Editing your local hosts file

Your local file contains many entries, do not edit those. Just add a new line with the IP of your App and the domain you want to see routed there like so:

```
# pattern (how it works)
[your App's IP address] [your fully qualified domain name]

# example (how it looks like)
12.0.0.1 mydomain.com www.mydomain.com
```

#### hosts file VS CNAME records

Please mind that you'll only check the routing with the IP of the App not the actua CNAME routing here. CNAME entries are not allowed in the hosts file.

#### Undo changes to your hosts file

After your domain has been moved/propagated be sure to remove the entry from your hosts file.




### Configuring your domain for e-mail

So far we have covered how to route a domain to fortrabbit. To receive and send e-mails from your domain you will configure the MX record of your domain. Please see your e-mail hosting provider for instructions.
