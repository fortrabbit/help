---

template:      article
reviewed:      2020-10-22
title:         Domain issues
naviTitle:     Domain issues
excerpt:       What you may want to know about our 404 errors.
lead:          'DNS is hard. This article aims helping you - the developer - solving common problems when connecting a domain to your fortrabbit App.'
group:         troubleshooting
stack:         all
dontList:      false

---


### Use the fortrabbit Dashboard as a guidance

When visiting your domain within the Dashboard, you'll see two tables: On the left you see the desired settings of the domain. On the right, you'll see the current, actual settings. 

In general — when not using an external DNS service like Cloudflare — those two settings should match. The left and the right side should be the same.


###  Wrong DNS settings

Please mind the difference between the naked and the `www.` domain. You have one domain registered with your provider, but from a DNS point of view, those things are different:

1. `www.mydomain.com` < the www-domain
2. `mydomain.com` < the naked domain

The DNS settings described here need to be applied on the right place:

* The IP address for the A-Record is for the naked domain ONLY. 
* The CNAME is for the www-domain ONLY. 

When registering a `www.` domain with fortrabbit we'll provide an IP address for an A-Record. This one is for the naked domain only. This IP will forward all requests to that domain to the `www.` version.

So, in general: When your `www.` domain has an A-Record, there might be something wrong. So when your naked domain has a CNAME record, there is definitely something wrong.

Please check the domain in the Dashboard > {{app-name}} > domains > {{domain-name}}. That view shows you two tables: The one on the left is the desired setup and shows two rows: one for the naked, one for the `www.` domain. On the right hand side you see the same table, but this one shows you the actual current settings. If these match, everything will work.

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

See also [here](/quirks#toc-outgoing-ip) for why you'll probably need your App's IP.

### Time delays

Many DNS providers default the TTL (Time To Live) of all records to 24 hours. This means that all changes will be applied with a delay of up to 24 hours, because everybody who has looked up the domain caches the result for the TTL duration. Also the TTL is not guaranteed: One badly programmed router on the way, who ignores the TTL or imposes its own minimal TTL, can change the TTL for everybody "behind it". The caching itself is actually a good thing as it helps to reduce round trips. Some providers let you set down the TTL, which is very useful when moving or changing domains.


### Testing domain routing with your local hosts file

Let's say you are developing a new App and want to use your custom domain before actually routing it to fortrabbit. Just add the domain to fortrabbit, as you would do with any actually routed domain, then modify your local hosts file, which lets your local machine know ahead of time that the domain is to be served from your fortrabbit App.

#### hosts file location

The hosts file is a text file (without file type ending). It can be found here:

* Windows: `c:\windows\system32\drivers\etc\hosts`
* MacOS & Linux: `/etc/hosts`


#### Editing your local hosts file

Your local file contains many entries: do not edit those. Just add a new line with the IP of your App and the domain you want to see routed there like so:

```
# pattern (how it works)
[your App's IP address] [your fully qualified domain name]

# example (what it looks like)
12.0.0.1 mydomain.com www.mydomain.com
```

#### hosts file VS CNAME records

Please bear in mind that you'll only check the routing with the IP of the App not the actual CNAME routing here. CNAME entries are not allowed in the hosts file.

#### Undo changes to your hosts file

After your domain has been moved/propagated be sure to remove the entry from your hosts file.
