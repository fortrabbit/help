---

template:      article
title:         "Quirks & constraints"
reviewed:      2023-11-15 15:55:21
naviTitle:     Quirks
lead:          "Limits, restrictions, permissions — aren't there always some? Heads up so it doesn't cost you hours of researching in the wrong direction."
stack:         all
group:         quirks

---

## Service scope

Apps are lightweight containers optimized for speedy web delivery of PHP applications. Apps are specialized to do just that. Apps are single-purpose by design. Apps are also managed.

* [Learn more about Apps](/app)
* [Learn about the differences to VPS hosting](https://www.fortrabbit.com/why-not-vps)

## No 1-click installers

When creating an App we ask for the desired software you are about to use. This can be Laravel, Craft CMS, WordPress or alike. **That will not install the software for you.** Please don't expect one-click-installers here. This is self service PHP hosting for professional developers. We believe in a clear separation of concerns where you will take care of the code you'll write and use and where we will manage the infrastructure. We also expect you to have a local development environment, see [the local dev article](/local-development).

* Read more more about the [Software Preset here](/app#toc-software-preset).
* Our [support policy](https://www.fortrabbit.com/support-policy) outlines service scope as well

## No root shell

Professional Apps only have [remote SSH execution](/remote-ssh-execution-pro). [Universal Apps](/app-uni) come with an SSH environment, but that is not a root shell, it's "jailed". So you can use it for deployment and for common tasks around development. Therefore, it's NOT possible to install software like: FFmpeg, Node, NPM, Gulp, webpack, ruby, Rails or mailserver. 

We do this for security and performance reasons. Embrace the idea of decoupled services, don't let your users wait, while your application is crunching a video or some images. Consider using an alternative or a third party service.

The Pro Stack has the [Worker Component](/worker-pro) to have CPU intensive long running tasks run in the background, but again only for software you can install via Composer.

### Need to call via php interpreter

To launch a PHP script you need to specify the PHP interpreter explicitly: for example, `php artisan` or `php craft`.

## No image optimization tools

Tools like `jpegoptim`, `optipng`, `pngquant`, `SVGO`, `gifsicle` and alike can help to reduce file size of your images. Using such tools are considered a best practice today. Unfortunately they consume a lot of CPU time and memory. fortrabbit Apps are build for fast short running light processes, not heavy lifting. Consider using a third party service for this.

## No video compressing

`ffmpeg` and `ffprobe` alike are also not available to avoid overuse of resources.

### wkhtmltopdf

wkhtmltopdf is a popular library to convert HTML to PDF. It's NOT installed and you can not install it on your own for the reasons named above. Check out the following alternatives: 

* [dompdf](https://github.com/dompdf/dompdf) is a PHP only PDF by CSS renderer
* Use PDF as a service like [cloudconvert](https://cloudconvert.com/) or [others](https://stackoverflow.com/a/5344424/1449386)
* Rethink if you really need PDF? An invoice in HTML is sometimes ok too
* Consider PDF creation on the client side with JS in the browser, with [jsPDF](https://parall.ax/products/jspdf) or similar

### Performance

Apps are designed for fast web delivery — to answer page requests swiftly. Unlike a VPS, which is a multi-purpose box, Apps are single-purpose for web delivery. Installing CPU intensive software would hurt the performance.

* [Learn about the resource limits here](/limits)

### Security

We believe in a clear division of security. In a nutshell: We - fortrabbit - take care of the Operating System level and the PHP runtime, you - the developer - are responsible for the software you write and use. Installing additional software would blur that strict division.

* [Learn about the security concepts here](https://www.fortrabbit.com/security)

## Service location

Data center locations are available in Ireland (AWS EU-1) and Virginia / USA (AWS US-EAST-1). A data center can be chosen for each App individually, but can't be changed later on. The service is available in Euro (€) or US Dollars ($): This can be chosen with each [Billing Contact](/billing-contact). The fortrabbit headquarters is based in Berlin, time zone is: CET.

## Mailing

Sendmail — the Mail Transfer Agent — is not available on fortrabbit.

In PHP Sendmail is usually used with the `mail()` function. Back in the good ol' days, this would simply call the shell command `sendmail` from the shell to send a mail directly from the web server to the receiving mail server.

In recent days, this is a really bad practice: your web server can send mails, but not receive any - it is not a mail server. So it is not distinguishable from any home PC part of a bot net. Long story short: it is very likely that your mail will not be received.

### Direct SMTP

There is no mailing built-in here. Instead of `sendmail` you can use a mail script that uses SMTP (Simple Mail Transfer Protocol) via your e-mail provider - usually the one that provides your domain - directly.

There are countless possibilities how to use SMTP. Most frameworks and CMS give them to you out of the box. If you use a custom script, have a look at [Swift Mailer](https://swiftmailer.symfony.com/). There are special solutions for [WordPress](install-wordpress#toc-smtp), [Laravel](install-laravel#toc-smtp) & [Symfony](install-symfony#toc-smtp).

A better solution might be to use a "transactional mail service", those are built to do the bulk mailing — see [extending fortrabbit](/#extending-fortrabbit). You can either connect to them via "SMTP relay" or by "API". Those services help you to save your App's resources, are probably more reliable and have some nice extra features like analytics and debugging.

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

The PHP extension `HTTP` from PECL is enabled by default. The classes `HttpRequest`, `HttpResponse` and `HttpMessage` are a very handy replacement for curl and the like. The downside is that it breaks CakePHP in some cases. You can disable the extension in your Settings -> Extensions tab of your App.

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

Only the branches `main`, `master` and the `{{app-name}}` will be deployed to the webspace. More [here](git-deployment#toc-the-branch-name-counts).

### Release package limit

To keep deployment fast for everyone the size of the release package is limited to 200 MB. [Read on](git#toc-release-package-limit).


## MySQL with persistent connections during upgrade

When scaling (up or down) your MySQL, your App's database will be migrated to a new node. Therefore the CNAME target of your App's MySQL hostname will be changed. The hostname's time to live (TTL) is 60 seconds, which reflects the maximum expected downtime during upgrade.

With persistent connections this can take longer (possibly up to half an hour). Therefore we recommend to disable persistent connections during upgrades and downgrades.


## Outgoing IP address

In some cases you need to know your App's IP address, like for payment processing or in environments that are behind a firewall.

**For [Professional Apps](/app-pro)** on production level plans, the outgoing IP address is fixed. All outgoing traffic from these Apps is routed through a NAT gateway. The traffic from Apps will appear to others as coming from these addresses:

    EU: 52.50.42.152
    US: 52.72.32.63

This is also true for [Workers](/worker-pro) but not during deployment.

**For [Universal Apps](/apps-uni)** and for Pro Apps on development plans the IP address is not guaranteed. Although it will most probably not change during the lifetime of an App. You can see this for yourself with a PHP script. Querying an HTTP service like that from an App is the easiest way to determine the current IP address. 

    <?php
    echo file_get_contents("https://ifconfig.co/");


Depending on your needs, it is possible to use an HTTP proxy provider like [QuoteGuard](https://www.quotaguard.com/) to get a static IP address. 

Finally, there is also the [official list of AWS IP ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) and the same list in [JSON](https://ip-ranges.amazonaws.com/ip-ranges.json) format.

Do bear in mind, the place where you shell into with `ssh` or push with `git` is called the deploy service. The IP address of the deploy service may change.

    deploy.eu2.frbit.com 52.212.159.57
    deploy.us1.frbit.com 52.200.130.83

We do not intend to change the IP address of the deploy services, but we do not guarantee it.

## What this isn't

Embrace the idea of decoupled services! We don't offer some classical hosting providers standards such as:

* Plesk, cPanel, ISPconfig, any classical hosting control panel
* e-mail hosting, e-mail address configuration
* any WebMail, so no RoundCube or SquirrelMail
* Domain ordering, top level domain registration services
* pre-installed phpMyAdmin, phpBB, Wikis …
* one-click installers

Our advice to you, the professional developer: consider abandoning your all-in-one old school hosting.
