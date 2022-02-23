---

template:    article
reviewed:    2022-02-23
title:       .htaccess
lead:        Browsing the docs here you will find lot's of reference to a mysterious invisible file called ".htaccess". What's that about? How can you make use of it?
naviTitle:   .htaccess
group:       deployment
stack:       all

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip

rank:     3

---


<!--

DO NOT USE THE .htaccess class for code blocks, it will break the $ from being copy/pasteable....

-->


## About .htaccess

`.htaccess` is a hidden file that usually lives in the web root folder of your code base. It enables altering the web server's configuration directives. `.htaccess` rules apply to all subdirectories. `.htaccess` is usually not excluded in `.gitignore` so it will be deployed alongside your code. Take care: htaccess is a sharp sword. With great power comes great responsibility.

### .htaccess on fortrabbit

Your fortrabbit Apps are running on the Apache web server. You can make use of `.htaccess`. Missing (remember it's hidden) and wrong `htaccess` directives are common issues. These sensitive defaults are set on the fortrabbit platform regarding .htaccess:

* Apache configuration syntax 2.2 and 2.4 are supported 
* GZIP compression is enabled per default
* Access on all `.ht*` files is disabled, so nobody can read your .htaccess


### .htaccess and your framework or CMS

When you are using a framework or a CMS, chances are high, that you don't need to wrangle with `.htaccess` at all, as that comes built-in. Here are the most used ones:

* **Laravel** comes with a [boilerplate htaccess](https://github.com/laravel/laravel/blob/master/public/.htaccess) you can extend
* **Craft CMS** comes with a [boilerplate htaccess](https://github.com/craftcms/craft/blob/master/web/.htaccess) you can extend
* **WordPress** [manages htaccess](https://codex.wordpress.org/htaccess) for you


## Using .htaccess

You will find examples in context on the different help pages here on fortrabbit. Here are a couple of common categories of usage: 


### Redirects

The most common use case for `.htaccess` is to re-write URLs with `mod_rewrite`. You can direct requests to a subdirectory, or specific domain, prettify URLs by omitting file endings, force https and much more.


#### Redirect all requests to https

**Force https!** There is no need for your application to be reached over a non-secure connection. Use `.htaccess` to redirect all `http://` requests over to `https://`. This is how:

```plain
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Proto} !=https
RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L,N]
```

**Order matters. This rule should be one of first rules, so right on top of your htaccess file** Please also note the X-Header part. Other code snippets you find elsewhere might not work here. This is because we are running our Apache behind a set of load-balancers. They are performing the HTTPS encryption and not Apache. Many CMS and frameworks are already offering convenient settings and configurations for this.


#### Redirect all requests to the primary domain

Once you've added a [custom domain](/domains) you may want to prevent requests to your [App URL](/app#toc-app-url). The example below shows how to set up a domain based redirect in your `.htaccess` file.

```plain
# From App URL to your domain
RewriteEngine On
RewriteCond %{HTTP_HOST} ^.*\.frb\.io$ [NC]
RewriteRule .* https://www.your-domain.example%{REQUEST_URI} [r=301,L,N]
```

A CMS will have it's own options for that. If you don't use a redirect, make sure to at least set the "canonical URL" to be on your primary custom domain so that that search engines know that there is only one content.


#### Force HTTPS for future visits with HSTS

This goes one step further than just forwarding requests, it tells the browser to not ever again accept any connection in the future on not secured domain to your App. In your `.htaccess` file you can add this line (in addition to the rewrite rule above):

```plain
<IfModule mod_headers.c>
Header always set Strict-Transport-Security "max-age=31536000"
</IfModule>
```

This will make your browser remember to always use the secured version of your App. It makes use of the "[HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)" policy and improves security by eliminating the risks of man-in-the-middle TLS-protocol-downgrade attacks. Be careful: setting this header will tell the browser never (or that is: until max-age) to use `http://` again. So if you later on decide to serve (parts of) your site using no encryption, all those clients (browsers) which saw the header will not comply and keep using `https://`.

### Authentication

You can implement a simple standard browser prompt password check for your website without any extra PHP magic with .htaccess. Hop over to our dedicated [HTTP Auth article](/http-auth) to learn on how.


### Control HTTP headers

HTTP headers are part of an Hyper Text Transfer Protocol request and response. HTTP headers are defining the operating parameters of an HTTP transaction. Use htaccess to modify these.


#### Cache-Control

Don't serve the same content to the same client twice! Control how the browser of the client caches results and files locally. This is especially useful for asset resources that don't change often. Caching reduces the number of request and the data transmitted. On the HTTP part of your App/website caching is achieved by using HTTP headers. You can control the caching in htaccess like so:

```plain
# Example to cache images and CSS files
# adjust and extend to your needs
<ifModule mod_headers.c>
  #  images expire after 1 month
  <filesMatch ".(gif|png|jpg|jpeg|webp|ico|pdf|svg|js)$">
    Header set Cache-Control "max-age=2592000"
  </filesMatch>
  # CSS expires after 1 day
  <filesMatch ".(css)$">
    Header set Cache-Control "max-age=86400"
  </filesMatch>
</ifModule>
```

If you are using a CMS this is usually already managed for you in PHP code. You can open the web inspector on your website, go to the network tab and check the headers for any images, to see if they already have cache headers.

Beware: Adding this also introduces the main problem of caching. If you later decide to change the image or css file, you must either wait the full cache time, or give the file a new name to make the browser retrieve it again. Otherwise it will use the old cached version.

#### CORS headers

For "Cross-Site XMLHttpRequests" you'll need "Cross-origin resource sharing" or in short CORS headers. Those are mostly used in context of JavaScript AJAX requests across different domains. Like when `domain-a.com` loads a script from `domain-b.com`. Per default this is not possible for security reasons. But you can enable it for certain or even all origins:

```plain
# Access all areas (use carefully)
Header add Access-Control-Allow-Origin "*"
Header add Access-Control-Allow-Methods: "GET,POST,OPTIONS,DELETE,PUT"
```

Use this with care and only open what you really need. Reduce the risk of XSS. Also check out the new Content Security Policy (CSP) for more advanced control on what you allow and what not. This website is a good reference: [content-security-policy.com](https://content-security-policy.com/)


### Environment detection

You might want some `.htaccess` rules only to be applied in a certain environment. A common use case is that is to forward all traffic to HTTPS on your fortrabbit App in production, but not in your local development environment. 


#### 1. Make sure environment variables are set

In the Dashboard with your fortrabbit App, make sure an environment variable key-value pair like this is present:

```.env
ENVIRONMENT=production
```

In some cases we have pre-populated that for you already. See also our [environment variables article](/env-vars) for more details on how environment variables are handled here.


#### 2. Setup .htaccess files

Now in the local code base of your project, setup a custom `.htaccess` file for each environment in the web root of your project: 

* `.htaccess` in general and for local development
* `.htaccess_production` for your fortrabbit App

This can also be extended to more environments. All `.htaccess` files should be under version control with Git.


#### 3. Configure Composer 

Add this to the script part of your `composer.json` in your local code base:

```json
"scripts": {
    "post-install-cmd": [
      "@php -r \" define('HTSRC', 'web/.htaccess_' . getenv('ENVIRONMENT')); echo (file_exists(HTSRC) && copy(HTSRC, 'web/.htaccess')) ? HTSRC.' copied to web/.htaccess'.PHP_EOL : ''; \""
    ]
}
```

This Composer post install command will replace the contents of the `.htaccess` file with the contents of the file `.htaccess_production`, if that file is present and the environment variable `ENVIRONMENT` is set to `production`.

On fortrabbit, each time you deploy with Git (see our [Git deployment article](/git-deployment)), composer install runs automatically (see our [Composer article](/composer)). 

You can extend the concept to have more stages of course (see our [multi staging article](/multi-staging)). To add custom rules for a staging environment, like http-auth (see [article](/http-auth)), create a matching `.htaccess_staging` file and set the `ENVIRONMENT` accordingly.

This also assumes that the root path of your App is `web`, which is true for Craft CMS. In the Composer example above, replace the `web` with the root path of your project, see an overview on different root paths [here](/app#toc-root-path).



### Allow access only for you

You may want to prevent accessing your website for anyone except your company. If your company has a fixed IP, you can use `.htaccess` to only allow traffic from that IP or range of IPs.

```
### 1st deny all

ErrorDocument 403 "Not Allowed"
Deny from all

### 2nd Allow your IPs 

Allow from 11.11.11.11
Allow from 12.12.12.12
```

An alternative to this is to use a password check with [HTTP Auth article](/http-auth).

### Custom error pages

You can define custom pages to make your error pages look more cool like so:

```htaccess
ErrorDocument 404 /404.html
```

This has to be a publically accessible URL. You can do this with 4XX and 5XX errors. When using a framework or CMS likely the router will catch such errors and use PHP logic to resolve it, still some 5XX errors might appear before the program can execute the router.


## Tips and tricks

### Comment your .htaccess file

Apache directives with regular expressions tend to look gibberish. Make sure to comment what you have been doing there so that "the later you" can understand what's going on.


## Troubleshooting

Here are the most common mistakes we see in combination with `.htaccess`:


### Missing .htaccess

Another common mistake we see quite often is to miss out the `.htaccess` file. The dot at the beginning of the file name indicates that this file should be invisible in your operating system. So when you dragged files in your FTP client from the Downloads folder, it's very likely that the `.htaccess` file was left home. This can cause 403 errors.

In macOS Finder you can toggle invisible files with ` CMD + SHIFT + .`, easy to remember, like dot files. SFTP clients with a split view option are mostly showing those files by default. Take care, there are possibly other hidden files in your project, like `.gitignore` and `.env`.


### Changes are not applied

`.htaccess` contents are cached (web browser), so it might take a while until you see changes you have made to `.htaccess` files to be applied. You can try `curl` to test the new rules or you can try another browser or incognito mode.


### Wrong location

The `.htaccess` file is usually placed in the [root path](/app#toc-root-path) of the website. Per default, that is the `htdocs` folder. But certain modern frameworks and CMS have path below that, like Laravel with `public`. So make sure to place your `.htaccess` in the correct directory. You can place the `.htaccess` files in folders below the root path as well, those will have a higher specificity and can override rules. 


## Possible performance issues

The fortrabbit hosting platform is managed. Therefore your access to critical services is limited. This is actually good for most parts. We believe in a clear separation of concerns: We run the servers, you do the coding. One consequence is that we can not give you direct access on the Apache web server configuration (httpd.conf).

The `.htaccess` file is a very common method to achieve common web server configurations. In fact, almost every framework and CMS comes with a `.htaccess` out of the box. `.htaccess` lives in your code base and is deployed along with Git and therefore it's just very convenient to use.  

Consider it a workaround that got too popular. The official Apache docs have a whole section on "[When NOT to use .htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html#when)". The NGNIX team has setup a marketing page [bad-mouthing .htaccess](https://www.nginx.com/resources/wiki/start/topics/examples/likeapache-htaccess/) for a bad design, performance-wise.

Now, usually we prefer clean, smooth and fast implementations. But sometimes one have need to make trade-offs. `.htaccess` brings convenience and security (separation of concerns). 

In our experience, the possible performance degradation can not be experienced.
