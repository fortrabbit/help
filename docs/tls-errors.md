---

template:      article
reviewed:      2022-03-21 17:25:06
title:         Troubleshooting TLS and HTTPS issues
naviTitle:     TLS troubleshooting
excerpt:       What you may want to know about our SSL/TLS errors.
lead:          Are you seeing a certificate error in the browser? This article aims to help developers troubleshooting such errors.
group:         tls
stack:         all
dontList:      false
order:         5

---


We provide free Let's Encrypt certificates for all domains. Additionally you can install your own. Read more about the HTTPS options in the [main article](/https).


### Review certificates in the browser

To troubleshoot TLS/SSL issues, it's often helpful to view the certificate in the browser. In Chrome and Firefox you can:

1. open https://www.{{domain}}.com/ < make sure to use https not http
2. click on the lock icon 
3. click on the "certificate" to reveal the cert


### You see a certificate warning 

You visit your Apps domain under the `https://` address and the browser throws an error that the certificate can't be verified. If you inspect the cert in the browser, you see that the cert is issued for `*.frb.io` not for your domain. The error might be:

```
NET::ERR_CERT_COMMON_NAME_INVALID
```

This can happen, if the domain is brand new and the cert is not yet installed. It can take up to 24 hours for the certs to get installed. The cert for the naked domain (for forwarding) usually takes a bit longer than the other one.

This can also happen, if your domain is not routed to fortrabbit (yet). Only domains that are already routed to fortrabbit will receive a Let's Encrypt cert. Please see the domain settings in the Dashboard. 

There are also other edge cases when this can happen, for example, if your domain [has set CAA records](/https#toc-secure-your-domain-with-a-caa-record) with DNS. 


### Cert is installed but browser bar is not showing a green lock

In most cases this is due to "**mixed content**", which means, the cert is installed and everything is working, but your website is requesting external resources over non-secure addresses (http). Check the source code of your website and find and replace all `http://` requests. You can use `https://` instead or you just leave out the protocol entirely like so: `//`. The last method will use whatever has been used before, so that works especially well, with different environments, for instance when your local development machine doesn't have TLS.


### It's not working with Internet Explorer 8 or older

Sorry, IE8 is not supported any more. All our TLS implementations are based on SNI.


### Solve SSL verification errors

This is the fix for verification errors when using ssl_verify. If you are receiving an error like the following:

```
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed
```
You need to set the `capath` as well. Assuming you are using PHP streams, it would look like this:
```php
$context = stream_context_create();
stream_context_set_option($context, 'ssl', 'verify_peer', true);
stream_context_set_option($context, 'ssl', 'capath', '/etc/ssl/certs'); # <<< that's the one
stream_context_set_option($context, 'ssl', 'allow_self_signed', false);

$fp = stream_socket_client("thedomain.tld:443", $errno, $errstr, 5, STREAM_CLIENT_CONNECT, $context);
# ..
if (stream_socket_enable_crypto($fp, true, STREAM_CRYPTO_METHOD_SSLv3_CLIENT) === false) {
    die("Failed to verify certificate");
}
#...
```

For cURL, the option `CURLOPT_CAPATH` needs to be set to `/etc/ssl/certs`.