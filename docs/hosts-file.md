---

template:      article
reviewed:      2021-09-06
title:         Testing domain routing with your local hosts file
naviTitle:     Hosts file for domain testing
excerpt:       Test domain setting before you route it
lead:          ''
group:         tips
stack:         all
dontList:      false

---

Let's say you are developing a new App and want to use your custom domain before actually routing it to fortrabbit. Just add the domain to fortrabbit, as you would do with any actually routed domain, then modify your local hosts file, which lets your local machine know ahead of time that the domain is to be served from your fortrabbit App.

### hosts file location

The hosts file is a text file (without file type ending). It can be found here:

* MacOS & Linux: `/etc/hosts`
* Windows: `c:\windows\system32\drivers\etc\hosts`

### Editing your local hosts file

Your local file contains many entries: do not edit those. Just add a new line with the IP of your App and the domain you want to see routed there like so:

```bash
# pattern (how it works)
[your App's IP address] [your fully qualified domain name]

# example (what it looks like)
12.0.0.1 mydomain.com www.mydomain.com
```

### hosts file VS CNAME records

Please bear in mind that you'll only check the routing with the IP of the App not the actual CNAME routing here. CNAME entries are not allowed in the hosts file.

### Undo changes to your hosts file

After your domain has been moved/propagated be sure to remove the entry from your hosts file.
