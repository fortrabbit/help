---

template:      article
reviewed:      2024-02-15 08:21:10
title:         Using HTTPS with fortrabbit
naviTitle:     HTTPS on fortrabbit
excerpt:       All about HTTPS and TLS.
lead:          Criminals and government agencies are trying to read your Apps communication. Better serve all requests over HTTPS for extended privacy. 
group:         tls
stack:         all

keywords:
    - ssl
    - TLS
    - domain
    - subdomain
    - CA
    - Certificate Authority
    - port 443
    - port 80

---


**TLDR;** Free and zero-config SSL certificates for all domains are provided, nothing you have to take care of. Read on if you want to learn about advanced options.


## About HTTPS, TLS & SSL

`HTTPS` is **H**yper **T**ext **T**ransfer **P**rotocol over Transport Layer **S**ecurity. It is used to secure the data transport between a client (browser) and a server. HTTPS is based on **TLS** (Transport Layer Security), which is the successor to the still better known **SSL** (Secure Sockets Layer). 

Current browsers show a green lock in the address bar if the connection is over HTTPS, the certificate could be verified and if the certificate is still valid. Google is promoting "https everywhere" (snakeoil), your SEO pagerank might benefit from using HTTPS.

## HTTPS options on fortrabbit

As part of [security efforts](https://www.fortrabbit.com/security) our aim is to give you secure, still easy-to-use and affordable options. Available with all Apps on fortrabbit:

### 1. Piggyback HTTPS

That's the most easiest and most obvious way to quickly run your App on https. You can already access your [App URL](app#toc-app-url) using HTTPS. For example: `https://{{app-name}}.frb.io`. Use this URL for testing or if a custom domain is not important to you.

### 2. Automatic HTTPS

[Let's Encrypt](https://en.wikipedia.org/wiki/Let's_Encrypt) is a free certificate authority and can be used to generate free certificates for custom domains. The Automatic HTTPS feature is included for all fortrabbit Apps: All domains correctly added and routed will receive an SSL certificate by Let's Encrypt, no configuration or extra setup required. The certs will also be renewed automatically. This option does not work for wildcard domains (yet).

### 3. Custom HTTPS

Bring your own certificate: There are various use-cases to mandate a custom certificate, for example if you plan on using wildcard domains or need a green browser bar with your name > using a [higher trusted](https://en.wikipedia.org/wiki/Extended_Validation_Certificate) commercial certificate.

#### How use Custom HTTPS

The roadmap to setup TLS custom reads like so:

1. have an external [domain](domains) registered with your fortrabbit App
2. create a key and a certificate request locally (see below)
3. using the certificate request: purchase an TLS/SSL cert from a third party CA
4. (book the TLS component for your App in the Dashboard) < Pro App only
5. upload your key and cert(s) to the Dashboard:  
App > Settings > All settings > Custom TLS certificate
6. route all your (sub)domains to your App

#### Create a new key and certificate

To obtain a certificate, you first must create a certificate signing request (CSR). With the newly generated CSR, you can go to an external certificate vendor, which will issue a certificate for you. For Mac/Linux you can use `openssl` from the the command line like so:

```bash
openssl req -new -nodes -keyout {{app-name}}.key -out {{app-name}}.csr -newkey rsa:2048
# Generating a 2048 bit RSA private key
# ..........................................................................................++
# .............................................+++
# writing new private key to '{{app-name}}.key'
# -----
# You are about to be asked to enter information that will be incorporated
# into your certificate request.
# What you are about to enter is what is called a Distinguished Name or a DN.
# There are quite a few fields but you can leave some blank
# For some fields there will be a default value,
# If you enter '.', the field will be left blank.
# -----
# Country Name (2 letter code) [AU]:DE
# State or Province Name (full name) [Some-State]:Berlin
# Locality Name (eg, city) []:Berlin
# Organization Name (eg, company) [Internet Widgits Pty Ltd]:fortrabbit
# Organizational Unit Name (eg, section) []:Website
# Common Name (e.g. server FQDN or YOUR name) []:www.fortrabbit.com
# Email Address []:info@fortrabbit.com
# -----
# Please enter the following 'extra' attributes
# to be sent with your certificate request
# A challenge password []:
# An optional company name []

# Update key format
openssl rsa -in {{app-name}}.key -out {{app-name}}.rsa.key
```

Do not enter a password! Also: if you plan on using `www.yourdomain.tld`, don't miss the `www.` in the "Common Name"! When you have a wildcard domain, use `*domain.tld` as the Common Name.

#### Checking your CSR

You can decode the created CSR file to make sure did it correctly. Run this in your local Terminal:

```
openssl req -in yourcsr.csr -noout -text
```

Substitute `yourcsr.csr` with your file. Or just use an online tool (CSR decoder).


#### Convert an existing key to RSA format

If you already have a private key and it begins with `----BEGIN PRIVATE KEY----` you need to convert it to the RSA format, using:

```bash
openssl rsa -in {{app-name}}.key -out {{app-name}}.rsa.key
# writing RSA key
```

The RSA format is required by our TLS implementation.

#### Finding the right Certificate Authority provider

Unsure from where to book your certificate? Most Certificate Authority provider look slightly dusty, but they do the job: [namecheap SSL certs](https://www.namecheap.com/security/ssl-certificates.aspx), [DigiCert](https://www.digicert.com/), [Thawte](https://www.thawte.com/), [GeoTrust](https://www.geotrust.com/), [SSLs](https://www.ssls.com/). We have NO business relations here.


#### Renewing certificates

Remember that certificates are usually only valid for one year. So you need to re-purchase a new certificate from your provider and re-install it with your App, when using Custom HTTPS . The steps are the same as described above.


## Using TLS/HTTPS

The following practical tips on how to deal with HTTPS are applying to all TLS options on fortrabbit:

### Redirect all requests to HTTPS

It is recommended to forward all requests to the secure line, so no more "http://", only "http**s**://". See [an example here](/redirects).


### Secure your domain with a CAA record

A Certification Authority Authorization (CAA) is a DNS record to specify which certificate authorities (CAs) are allowed to issue certificates for a domain. It's an extra security layer, so that now one else can intercept any certificates by wrong authorities. There is no integration here on fortrabbit needed. See if your DNS / domain provider supports CAA entries, set the according identifying domain name. When you are using our free Let's Encrypt certs, see [this article](https://letsencrypt.org/docs/caa/) on how to set it up. Mind that already existing CAA entries can also become a problem when trying to issue new certificates. 


## Troubleshooting TLS

Sometimes it just doesn't work as supposed to. Don't panic. You can of course always ask us for support. [Here](/tls-errors) are some things you can do on your own.
