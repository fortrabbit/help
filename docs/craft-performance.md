---

template:         article
reviewed:         2023-01-19 14:19:41
title:            Craft CMS performance tips
naviTitle:        Craft performance tips
lead:             All about performance problems with Craft CMS and how to run Craft CMS fast on fortrabbit.
group:            craft
stack:            all
order:            100

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-mark-black-new.svg
version:          4.0
supportLevel:     a

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

rank: 100

---

## What you want to know about PHP performance

### Understand Time To First Byte and PHP response time

For web server performance "PHP response time" is an important metric. It can be roughly translated to the Time To First Byte ([Wikipedia](https://en.wikipedia.org/wiki/Time_to_first_byte)), which also includes network latency effects.

When running Craft CMS on fortrabbit, with a little tuning and attention to best practices, you should be able to attain a "PHP response time" of 250ms or even less.

### Understand PHP processes and execution time

Please see our dedicated [PHP processes article](/php-processes).

## How to identify performance problems

### Experience it yourself

Your website is slow, you will feel it. Common signs of performance issues are:

- Pages are slow to load - the browser loading icon spins
- You see timeout errors - a 504 error printed on screen

### Check the hosting metrics

The fortrabbit Dashboard offers a metric section (your hosting provider likely will have something similar) including the following vital data points:

- **PHP response time**: Aim for less than 250ms on average. In our experience, it's more likely that a website with an average high repsonse time will have problems. Slow websites that are getting some more visits than usual are tending to break down (504) faster.
- **5xx error metric**: See if there are any peaks in 5xx errors. If there are, see if at the same time, the PHP response time went up, in that case those 5xx errors are likely 504 time out errors.
- **Memory usage**: The memory used should not come to close to the hosting resources you have booked on average, 80% average usage and single peaks are Ok.
- **Memory swap usage**: Swap is when there is no (fast) RAM available anymore and data needs to be accessed from disc (slow). This should be within bounds, which depends on your website.
- **OpCache**: This should not max out. Aim for 80% or less.

### Use the Craft Debug Toolbar

The YII toolbar is integrated with Craft CMS. It ([see article by NYS+107](https://nystudio107.com/blog/profiling-your-website-with-craft-cms-3s-debug-toolbar)) provides useful information about which queries you run and how long it takes to execute them. Rule of thumb: if you run more 50 queries you should do something about it. It's easy to use and can help you to find slow queries with low efforts while developing.

- [Yii debug toolbar documentation](https://www.yiiframework.com/extension/yii-debug-toolbar)

### Use load testing

We advise to put your website under some controlled stress to see when it will break. There are tools you can use from your computer to fire up a couple of simultaneous requests. This way you can see how the website will behave before your first unlucky visitors will find the bottleneck for you.

### Use external profilers

There are also commercial, professional profiling services providing helpful detailed low-level profiling information. They can be useful for bigger projects with some more traffic. On the server side, they are integrated as PHP extensions (pre-installed on fortrabbit). The most popular ones are Blackfire (recommended by us) and NewRelic.

- [fortrabbit NewRelic help](https://help.fortrabbit.com/blackfire)
- [fortrabbit Blackfire help](https://help.fortrabbit.com/new-relic)

## Kind of issues

### MySQL related issues

The amount and complexity of database queries, as well as the size of your dataset, have a direct impact on the performance of your site. While the queries you create in your templates are more or less your responsibility, there are other queries that you can't directly control. We have analyzed hundreds of Craft sites to understand common query patterns. Especially sites with large amounts of data (Entries, Fields, etc) and frequent content updates suffer from these "hidden" queries.

#### How Craft CMS makes use of the database

The beauty about Craft CMS is that you can get by with just writing TWIG templates — you don't need to write a single MySQL query yourself. Within the TWIG templates there is an abstraction layer to create a query to the database. That's a dangerous tool at your proposal since there a couple of important not so well known things about it.

- Craft is running database queries all the time, completely blocking
- Craft is joining all the time, since everything is an element, that can get slow quickly

There is much more to know, exceeding the scope of this post. Here are the most important parts:

#### How the MySQL dataset size matters

The flexible content model in Craft CMS makes it easy to write code that makes either too many database queries and/or slow queries in MySQL. Most commonly poorly performing MySQL queries are easy to miss during development, since with local development you commonly **only have a small dummy dataset**.

Also mind your local machine has a different hardware than your hosted website. In production later on, when all the pages are published and a couple of blog posts have been written, the underlying issues will become more visible. MySQL query runtime is not just adding up, it's multiplying.

#### General tips on MySQL performance with Craft CMS

**Prevention is better than cure!** If you're reading this, the odds are you may already have run into performance issues on a Craft CMS project. But if you're just starting out with a new project, being aware of how Craft models content behind the scenes can spare you a lot of time-consuming debugging later on.

Look out for code smell: Common Craft performance anti-patterns include:

- `where` conditions on custom fields
- Queries in nested loops (N+1)
- Order on custom fields `orderBy`

Please also see our [fortrabbit general help on MySQL debugging](https://help.fortrabbit.com/mysql-performance).

**Optimizing database queries**

- [Official Craft documentation on Eager loading](https://craftcms.com/docs/3.x/dev/eager-loading-elements.html)

**Database indexes**

- [How does database indexing work? on StackOverflow](https://stackoverflow.com/questions/1108/how-does-database-indexing-work)
- [What columns are indexed in Craft (source code link)](https://github.com/craftcms/cms/blob/d4be41dae683f1f06d42c300620b556c9c057ad3/src/migrations/Install.php#L754-L935)

#### Use the Relax plugin

Fortunately, there is a plugin called [Relax](https://plugins.craftcms.com/relax) that takes care it. We have released it for free to help our clients and the broader craft community.

### Issues related to blocking PHP requests

Like described above, a PHP process is busy as long as it is executing. No one will pick up the phone (return a web page) when all the PHP processes are busy. There are various reasons why PHP requests are busy or are running for too long.

Examples from support:

- A plugin is querying an external service in a blocking way and the answer is taking too long
- The database is overwhelmed by too many or too expensive queries

### Memory/CPU related issues

Sometimes there is just not enough memory or computing power to perform a calculation. This is often the case when image transformations are involved or when a lot of concurrent requests hit your App. 

Examples from our support:

- Image transformations are used extensively to create too many versions of a certain image in too many sizes and formats
- Thousands of queue messages need to be processed, often caused by bulk-updates
- Bots crawl the entire site including non-existing pages which are not cacheable

### Craft queue related issues

Craft has its own queue implementation. You can see and control it from the Craft CMS Control Panel. The [craft-async-queue plugin](https://github.com/ostark/craft-async-queue) helps to improve the performance of the Craft queue. Hanging queues are a bad sign.

To see if performance problems are related to the Craft queue, check if one is running and delete it from the Craft Control Panel. This of course is only possible when the website is still responding.

Look for errors in the Craft queue.

### Disk I/O related issues

Sometimes the file system disk operations are slowing down a website. This can be when your website is accessing the disk a lot.

Examples from support:

- Using Craft's Twig `{% cache %}` tag missing the key attribute can lead to thousands of cache files with the same data and a terrible cache-hit-rate
- The website is configured to write verbose log files

#### Static files in /templates

We've seen this many times. Developers put static assets like `.css` and `.js` in the `/templates` folder next to the twig templates. Craft delivers those files, but it creates massive overhead. Instead you should always put static files in `/web/some-dir/` to prevent any PHP execution when delivering those files.

#### Twig cache tag

Craft's `{% cache %}` tag is a good thing to prevent execution of expensive queries over and over again, however it's **very important** to use it wisely. Make sure to use a specific cache key, otherwise you risk an inefficient Cache-Hit-Rate and lots of cache items filling up the disk or Memcache.

### General configuration related issues

Performance problems are also often caused by general misconfiguration.

Examples from support:

- Not having set the environment to `production` - this can contribute to bad performance since in development mode more things are getting logged
- A plugin is (mis)configured to blow up the database

## Fixing and mitigation

In our experience, problems are often unique to individual projects. So there is no silver bullet to make things fast. There are however some general things you can do:

### Check and maybe block certain requests

Depending on your configuration, checking the source of requests might help you to understand where resources are getting spent. 

Examples from support:

- A deep content structure, creating an endless amount of possible pages that are getting crawled by web crawlers. Block certain bots or rethink your content structure.
- Common bot attacks targeting WordPress requesting a `wp-login` page which results in a 404 page, but that page is not cached or creating an expensive database query, so that the requests are generating a lot of load.
- The Blitz plugin is configured to flush the cache every hour, which triggers cache warming. [See this Twitter thread](https://twitter.com/o_stark/status/1189626958713368578).

### Caching to the rescue

Caching can be highly beneficial, particularly in providing the fastest possible end user experience but shouldn't be a crutch. Instead, it's usually better to find and remediate the **root cause** of the issue where possible. 

Doing caching wrong can also be the problem itself.

You want repeating parts of the website or full pages to be cached and rendered without hitting PHP or a database query touching the server. A "mega menu" is a perfect example and an opportunity for fragment caching. It creates the same database queries (sometimes 50+) for every page.

There are multiple approaches for caching in Craft CMS including:

- [**Native Twig cache tag**](https://craftcms.com/docs/3.x/dev/tags.html#cache) - the out of the box tool for fragment caching, be careful when using for side effects, more above.
- **[Blitz](https://github.com/putyourlightson/craft-blitz)** Craft CMS plugin - a popular full cache plugin with tons of options
- **[Upper](https://github.com/ostark/upper)** Craft CMS plugin (by fortrabbit co-founder Oliver Stark) - integrates reverse proxies (Cloudflare, Vanish, KeyCDN) with Craft

Please be careful about caching. In our experience, this is probably causing more problems than it is solving.

### Book more hosting resources

Of course, we - as our hosting provider - should have an interest in selling you bigger servers, but our experience shows that it usually needs a lot of money to compensate coding and config mistakes and is usually also not a good fix. You can try shopping more PHP memory, better CPU power, and more PHP process.

For Craft CMS we recommend at least our current Standard Plan including 256 MB of RAM for PHP.

## Further reading

### Craft Quest videos

Ryan has some good educational video content (some paid, some free) on related topics:

- [About the Yii debug toolbar](https://mijingo.com/lessons/yii-debug-toolbar-craft-cms/)
- [Debugging in Twig and Craft](https://craftquest.io/lessons/debugging-in-twig-and-craft)
- [Profiling with Xdebug live stream](https://craftquest.io/livestreams/profiling-in-xdebug)
- [Debugging with Xdebug course](https://craftquest.io/courses/debugging-with-xdebug)
