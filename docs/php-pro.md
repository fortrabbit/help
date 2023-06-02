---

template:      article
reviewed:      2023-06-02 10:31:13
title:         Using and scaling PHP
naviTitle:     PHP
lead:          PHP is — as you are probably aware — a popular web programming language. It's also the core Component here on fortrabbit. See here how to use and scale PHP.
group:         Components
stack:         pro

keywords:
    - app-design

---


<!-- TODO: merge (maybe partly) with scaling-pro article (to similar topics) -->

## Booking PHP

When you create a new [App](/app) in the [Dashboard](/dashboard) you'll be presented with a plan chooser for the PHP Component. See below to select the correct initial scaling.

## Configuring PHP

You'll find a PHP settings page with your App in the Dashboard. Here you can switch the PHP version and configure various PHP extensions.

## Vertical scaling

When first creating your App, you might or might not know how much memory (PHP memory limit) your application will need. The amount determines the [vertical scaling](scaling-pro#toc-vertical-scaling) plan you should choose.

We offer three main PHP scaling plan sizes: **PHP s, PHP m, PHP l**. Which of those fits your App best depends on the technology stack (framework, CMS …) and on the code you write. For the latter you should read our [application design](app-design) guide on how you can get the most performance out of your application.

Don't be afraid to test and experiment with different scaling settings: You can instantly scale up and down to find the best match. Here are our recommendations to get started:

### PHP s: universal usage

The majority of applications will run within this scaling group.

* [Symfony](http://symfony.com/), [our install guide](install-symfony)
* [Laravel](http://laravel.com/), [our install guide](install-laravel)
* [WordPress](https://wordpress.com/), [our install guide](install-wordpress)
* [Craft CMS](https://buildwithcraft.com/), [our install guide](install-craft)
* [Drupal8](https://www.drupal.org/), [our install guide](install-drupal)
* [CodeIgniter](https://www.codeigniter.com/)
* [Phalcon](https://phalconphp.com/en/), [our install guide](install-phalcon)
* [Grav](http://getgrav.org/), [our install guide](install-grav)
* [Slim](http://www.slimframework.com/), [our install guide](install-slim)

### PHP m: bigger CMS & grown Apps

* [eZ](http://ez.no/)
* [Neos](https://www.neos.io/)
* [Zend](http://framework.zend.com/)
* your Composer requirements grow beyond 30 lines

### PHP l: fat CMS & e-commerce

* [Magneto](http://magento.com/)
* [Sylius](http://sylius.org/)
* [Thelia](http://thelia.net/)

### When to upgrade the vertical plan

Once you have found the perfect vertical size you usually don't need to change it. However, if your App grows strongly in terms of increasing functionality (i.e. code size) or if your code does not scale well with increasing amounts of visitors, then you might need to upgrade the vertical size.

To be ready for that: watch the memory and swap metrics in the [Dashboard](dashboard): if the memory is maxed out and you are seeing an increasing swap usage then your App needs more memory.

In some cases swap usage by the application is not possible. In these cases you might see dreaded white-pages and along going error messages in the log: "Allowed memory size of 1234.. bytes exhausted (tried to allocate 234 bytes)." This should be understand as an urgent reminder to scale to the next vertical size.

## Horizontal scaling

You can [scale horizontally](scaling-pro#toc-horizontal-scaling) for two reasons: 1) higher availability, 2) more visitors. While single Node Tinkering plans can handle low traffic amounts easy enough, multi Node Production plans can handle live traffic and come with considerable increased availability.

If the App is mission critical, then you want to consider using plans which could handle more traffic than it is actually getting. More Nodes assure that the App keeps running without any impact even of one of the Nodes fails (although it would automatic recover, it could take some minutes).

## Scaling from Tinkering to Production

Tinkering plans provides a good starting point to get the App setup. Once the App goes live and availability and performance become a consideration scaling to Production plans is recommended.

### DNS

Tinkering level plans offer a simplistic routing without a load balancer. When scaling up from a Tinkering to Production the App will move behind load balancer, to assure it's ready to run on multiple Nodes. This means an internal DNS change. As a consequence, you should plan your upgrade to Production level beforehand and make sure to calculate a 1 to 5 minute interval of DNS re-routing, in which the App might be not responsive.

Upgrades between Production levels are smooth and completely transparent to any visitors since they stay on the same load balancer.

### Memcache is recommended

When using Production PHP plans, which run on two or even more Nodes, you need a solution to store session and cache data in a way that it can be accessed from all Nodes. Our recommendation is to use the network cache [Memcache](memcache-pro) for this. It's fast and easy to use.

Alternatively you can store session data in the [MySQL Database](mysql) and cache data in APCU, which works fine for Apps with few visitors (~1k PHP requests per hour) in our experience.

## Scaling from Production to Dedicated

The Dedicated PHP plans are the continuation of the Production level. They allow your App to grow the number of PHP requests to a really large number. To provide the best fit for your App they come with an additional setting in the Dashboard: "FPM process processes per Node". You have three options, which map to our [main vertical PHP plan sizes](#toc-vertical-scaling):

* If you come from PHP s: Choose small processes
* If you come from PHP m: Choose medium processes
* If you come from PHP l: Choose large processes

You (might) have 3 GB Memory for your FPM processes. Now with this setting you can define how to use those. When you set this to 10, each process will have 300 MB. When you set this to 20 each process will have 150 MB. The best setting is defined by the type of your application (see [above](#toc-vertical-scaling)). Less is better of course, so you can run the App on more processes. More processes enable the App to "process" more parallel requests at the same time — handle more traffic. Our memory metrics in the Dashboard help you to see the memory usage of your App.

Each dedicated PHP plan comes with a dedicated load balancer cluster, which we manage and which will scale automatically with your Apps demands.

## PHP processes

See the dedicated [PHP process article](php-processes).

## PHP memory

The memory of each PHP plans is only for PHP usage - no Apache, no MySQL, no nothing. So 128 MB, 256 MB and even 512 MB of memory might sound small, but it is actually quite a lot, because it is not shared with other resources.

How much memory your App will require depends on multiple factors. Let's start with the theory:

**Extensions**: Each enabled PHP extension must be loaded into memory. Extension have a different size, so a full fledged framework such as Phalcon uses more memory than a simple driver such as Memcached.

**OPcache 1: PHP libraries**: You most likely are using Composer to manage your dependencies. This implies a set of PHP files which need to be interpreted and loaded into memory - specifically into the OPcache.

**OPcache 2: PHP code**: The code you write yourself consumes memory in the OPcache as well. The more code you have, i.e. the more functions and complexity, the more memory is required.

**Runtime variables**: In each request your App fills variables (`$foo = "bar"`), which consumes memory. How much it will use depends on the amount of data used in variables. You can measure how much you are by calling [memory_get_peak_usage()](http://php.net/manual/en/function.memory-get-peak-usage.php).

Now, the memory used by extensions and OPcache is shared. This means: If you run an App on a single process and use 10 MB OPcache, then you will also only use 10 MB of OPcache with two processes. The memory consumed by runtime variables is not shared.

It's hard to pre-calculate the expected memory usage of your App. However, you can make educated guesses.

For example, using [Slim Framework](install-slim) does require (big surprise) far less memory than using a full scale framework such as CakePHP or [Laravel](install-laravel). A form + email based mini-eshop requires far less memory than a Magento installation with plugins.

An approximation, although with some exceptions, is to look at the size of the source code to guesstimate the expected memory usage. Using the same example: the source code (especially including Composer package files) of a Laravel installation is larger than that of Slim Framework, so you can expect Laravel's memory consumption to be larger, and you would be right.

The tricky part is the runtime data. Say you have a blog with comments using a database. Now rendering a small blog entry with no comments will utilize less memory than rendering a large blog entry with thousands of comments. Or does it? Well, it really strongly depends on your design and actual code. For example, you could load all of the thousands of comments into a PHP array and then render the site or you could load them one by one and print them out immediately - both would have a different impact on memory. In general the memory usage will change over the lifetime of your App - expect it to increase with growing data size.

## Further reading

Please also see the [general scaling article](scaling), which explains concepts such as vertical and horizontal scaling and gives you a broader overview.
