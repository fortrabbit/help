---

template:      article
title:         "Quirks & constraints"
reviewed:      2019-10-01
naviTitle:     Quirks
lead:          "Limits, restrictions, permissions — aren't there always some? Heads up so it doesn't cost you hours of researching in the wrong direction."
stack:         all
group:         platform

---

## Service scope

Apps are lightweight containers optimized for speedy web delivery of PHP applications. Apps are specialized to do just that. Apps are single-purpose by design. Apps are also managed.

* [Learn more about Apps](/app)
* [Learn about the differences to VPS hosting](https://www.fortrabbit.com/why-not-vps)


## No root shell

Professional Apps only have [remote SSH execution](/remote-ssh-execution-pro). [Universal Apps](/app-uni) are coming with a SSH environment, but that is not a root shell, it's "jailed". So you can use it for deployment and for common tasks around development. Therefore, it's NOT possible to install software like: FFmpeg, Node, NPM, Gulp, webpack, ruby, Rails a mailserver. 

We do so for security and performance reasons. Embrace the idea of decoupled services, don't let your users wait, while your application is crunching a video or some images. Consider to use an alternative or a third party service.

The Pro Stack has the [Worker Component](/worker-pro) to have CPU intensive long running tasks run in the background, but again only for software you can install via Composer.

## Image optimization tools

Tools like `jpegoptim` and `optipng` can additionally help to reduce file size of your images and are considered a best practice today. As much as we would like to make those tools available, we can't. They consume a lot of CPU time and memory. fortrabbit Apps are build for fast short running light processes, not heavy lifting. 


### wkhtmltopdf

wkhtmltopdf is a popular library to convert HTML to PDF. It's NOT installed and you can not install it on your own for reasons named above. Check out the following alternatives: 

* [dompdf](https://github.com/dompdf/dompdf) is a PHP only PDF by CSS renderer
* Use PDF as a service like [cloudconvert](https://cloudconvert.com/) or [others](https://stackoverflow.com/a/5344424/1449386)
* Rethink if you really need PDF? An invoice in HTML is sometimes Ok too
* Consider PDF creation on the client side with JS in the browser, with [jsPDF](https://parall.ax/products/jspdf) or alike


### Performance

Apps are designed for fast web delivery — to answer page requests swiftly. Unlike a VPS, which is a multi-purpose box, Apps are single-purpose for web delivery. Installing CPU intensive software would hurt the performance. 

* [Learn about the resource limits here](/limits)

### Security

We believe in a clear division of security. In a nutshell: We - fortrabbit - take care of the Operarting System level and the PHP runtime, you - the developer - are responsible for the software you write and use. Installing additional software would blur that strict division.

* [Learn about the security concepts here](https://www.fortrabbit.com/security)


## Service location

Data center locations are available in Ireland (AWS EU-1) and Virginia / USA (AWS US-EAST-1). Data center can be chosen for each App individually, but can't be changed later on. The service is available in Euro (€) or US Dollars ($) this can be chosen with each [Billing Contact](/billing-contact). The fortrabbit headquarter is based in Berlin, time zone is: CET.

## Mailing

Sendmail — the Mail Transfer Agent — is not available on fortrabbit.

In PHP Sendmail is usually used with the `mail()` function. Back in the good 'ol days, this would simply call the shell command `sendmail` from the shell to send a mail directly from the web server to the receiving mail server.

In recent days, this is a really bad practice: your web server can send mails, but not receive any - it is not a mail server. So it is not distinguishable from any home PC part of a bot net. Long story short: it is very likely that your mail will not be received.


### Direct SMTP

There is no mailing builtin here. Instead of `sendmail` you can use a mail script that uses SMTP (Simple Mail Transfer Protocol) via your e-mail provider - usually the one that provides your domain - directly.

There are countless possibilities how to use SMTP. Most frameworks and CMS give them to you out of the box. If you use a custom script, have a look at [Swift Mailer](https://swiftmailer.symfony.com/). There are special solutions for [WordPress](install-wordpress#toc-smtp), [Laravel](install-laravel#toc-smtp) & [Symfony](install-symfony#toc-smtp).

Pro tip: In Gmail you need to allow "less secure apps" to connect. See the [official Google help](https://support.google.com/accounts/answer/6010255).

A better solution might be to use a "transactional mail service", those are built to do the bulk mailing — see [extending fortrabbit](/#extending-fortrabbit). You can either connect to them via "SMTP relay" or by "API". Those services help you to save your Apps resources, are probably more reliable and have some nice extra features like analytics and debugging.


## PHP

### PATH_INFO

Our runtime implements PHP via FastCGI + FPM. To utilize `PATH_INFO` you need to do a small hack-around in your `.htaccess` file:

```
RewriteCond %{REQUEST_URI} \.php/ [NC]
RewriteRule ^(.+)\.php/(.+)$    /$1.php [NC,L,QSA,E=PATH_INFO:/$2]
```

### Basic authentication

If you want to use [HTTP basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication), you need to forward the `Authorization` header via an `.htaccess` directive and parse the contents manually.

### Authorization header

If you need the `Authorization` header, for OAuth for example, you have to forward the header explicitly via an ENV variable:

```
RewriteCond %{HTTP:Authorization} .
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
```

### pecl_http

The PHP extension `HTTP` from PECL is enabled by default. The classes `HttpRequest`, `HttpResponse` and `HttpMessage` are very handy replacement for curl and alike. The downside: it breaks CakePHP in some cases. You can disable the extension in your Settings -> Extensions tab of your App.

### Available locales

<pre><code class="plain" id="locales"></code></pre>

<script type="text/javascript">
    $(function() {
        console.log("Loading locales");
        $.get('/locales.php', function(res) {
            $('#locales').text(res.sort().join("\n"));
        })
    });
</script>

## Git

### Branch name matters

You can use / create as many [branches](git) as you want and push them to the fortrabbit remote repository. However there are only two branches which will be deployed: the `master` branch and a branch which has the same name as your App. If your App is named `your-app` then a branch named `your-app` will be preferred over the `master` branch.

This is a feature, not a bug: use other branches as "transport" branches to interchange code with other developers / locations without publishing it to your web space. Once your code is ready to deploy, just merge it in the master (or your Apps name like: {{app-name}}) branch and push it.

### Release package limit

To keep deployment fast for everyone the size of the release package is limited to 200 MB. [Read on](git#toc-release-package-limit).

## MySQL with persistent connections during upgrade

When scaling (up or down) your MySQL, your App's database will be migrated to a new node. Therefore the CNAME target of your Apps MySQL hostname will be changed. The hostname's time to live (TTL) is 60 seconds, which reflects the maximum expected downtime during upgrade.

With persistent connections this can take longer (possibly up to half an hour). Therefore we recommend to disable persistent connections during upgrades and downgrades.



## Firewalling

Outgoing traffic is limited for [security](security) reasons — most ports for making outgoing calls are locked. Only the most [standard ports are white-listed](http://www.fortrabbit.com/specs#firewall). Any access to service Components provided by us directly (Memcached, MySQL, ..) is of course also allowed. Any other traffic is denied - but you can request to open ports for your App. To do so: login to the Dashboard, browse to your App, go to the firewall settings, "request a custom white listing".

<div markdown="1" data-user="known">
[Request a new firewall white-listing for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/firewall)
</div>



## Outgoing IP

In some cases you need to know your Apps IP address, like for payment processing or with fire-walling in corporate environments.

For [Professional Apps](/app-pro) on a production level plan the outgoing IP is fix, for each region (EU: `52.50.42.152`, US: `52.72.32.63`), except for the deploy service. For [Universal Apps](/apps-uni) and for Pro Apps on development plans the IP is not guaranteed. Although with high probability, it won't change during the Apps lifetime. 

You can setup a regular running "test", which queries https://ifconfig.co/ or the like to notify you on changes. Querying such a service from your App eg `<?php echo file_get_contents("https://ifconfig.co/");` is the easiest way to determine your Apps current IP. Depending on the use-case, it is possible to use a HTTP proxy provider like [QuoteGuard](https://www.quotaguard.com/) for a vanity IP address. 

There is also a [official list of AWS IP ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) also available in [JSON](https://ip-ranges.amazonaws.com/ip-ranges.json) which you can parse and use.


## What this isn't

Embrace the idea of decoupled services! We don't offer some classical hosting providers standards such as:

* Plesk, cPanel, ISPconfig, any classical hosting control panel
* eMail hosting, E-Mail address configuration
* any WebMail, so no RoundCube or SquirrelMail
* Domain ordering, top level domain registration services
* pre-installed phpMyAdmin, phpBB, Wikis …
* one-click installers

Our advice to you, the professional developer: consider to abandon your all-in-one old school hosting.
