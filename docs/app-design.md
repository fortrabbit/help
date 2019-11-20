---

template:      article
reviewed:      2019-10-24
title:         Application design & optimization
naviTitle:     Application design
lead:          Best practices: from development to production, from backend to frontend.
group:         platform
stack:         all

---

Of course: in case of a performance problem we can always throw more hardware on your App, but that won't always help when the problem is in the code somewhere. We want you to encourage to **design for performance** and scalability. And that is all about thinking a bit ahead.

## Backend design

The performance of a modern web application depends on the hosting environment AND the application design. In short: **We are in this together!** We — fortrabbit — as your hosting vendor, are responsible to provide a great, scalable and [secure](https://www.fortrabbit.com/security) infrastructure. You, as the developer, are responsible to leverage this infrastructure by good application design.


### Prepare to cache

If you begin developing, you might not instantly utilize a pair of multi gigabyte memcached servers. Still: make sure to implement caching early on and to use abstraction, so you can later on switch to those memcached machines when you need them. On fortrabbit you can use the following caches:

* **File**: Disk storage is not very fast and does not scale horizontally. It should be used for testing only!
* **Database**: Normally faster than disk. A good idea if you need to cache data for a long time (i.e. weeks).
* **APC**: All App plans include APC. Aside from opcode caching which happens automatically, you can use APC as an object cache very similar to _Memcache_. However APC lives in local memory, which is not shared between multiple Nodes.
* **Memcache**: Very fast, in-memory network cache, for Pro Apps.
* **Redis**: If you want to use Redis, just sign up with a third party Redis vendor and let us know the port they assigned to you.

A high cache hit rate indicates if your caching strategy works as expected. For APC and Memcache we provide Dashboard Metrics for _APC/Memcache misses_ (that's the hit rate from another angle). The rule of thumb in production: <10% misses is okay, <2% misses is better.

### Reduce I/O

It's always a good idea to reduce file input/output operations on the disk to the minimum. In PHP this means:

* **PHP includes**: Reduce amount of includes as much as possible. Always use absolute path names, which reduces the amount of lookups.
* **Avert files**: Don't use file-sessions, file-caches or anything `file-*`, if you can replace it with an in-memory cache or even the database.
* **Outsource static files**: Put your static files on an object storage. Sure, they could be delivered very fast from local, but still, they will create I/O which your (PHP) App will be missing.
* **Shrink your loader chain**: Make sure you load only the classes (files) you need. For a production App (in which code changes do not occur often) you can set `apc.stat` or `pcache.validate_timestamps` to `0`, which reduces I/O on `.php` files even further, but requires an APC/OPcache flush for changed files to apply.

### Decouple, but loosely

Leverage the power of the cloud: Span your App across multiple services. If you need image or video encoding: use an external service for this. If you want to send mass mails: use an external service for this. Why? Because it balances the load of your App and thereby gives it more resources for each individual task it has to perform.

However, if one of those external service (temporarily) breaks, make sure you can switch it off in seconds! You still want your shopping basket to run, if your image transformation has a problem.

### Scale out, not up

Up-scaling (bigger boxes) might be simpler, but with out-scaling (more boxes) you can grow nearly limitless. To be able to scale out means to be able to run your App across multiple nodes. Make sure you don't use local files or APC - or at least use abstraction, as mentioned above, so you can replace them transparently. Also, using multiple Nodes to deliver your websites comes with a feature you don't want to miss: high-availability. Horizontal scaling is available for [Pro Apps](/app-pro).

### Prepare for CDN

If you're planning on growing large and expect a lot of traffic, you will end up putting your static files (images, videos, css and js files, ...) behind a Content Delivery Network. Nowadays, you can do this pretty effortless with specialized services. Some services work best with a dedicated (sub)domain for your static contents, such as _img.yourdomain.com_. If you expect to use them in the future, you should implement the required structure from the start. Also a good precaution is to setup your cache headers properly from the start.

### Database considerations

Well, talking about database optimization could easily fill a whole book — or books. The most basic tips for relational databases, such as MySQL:

* **Smart indexing**: First off: Use indexes. But which? Look at your queries. MySQL provides [EXPLAIN](http://dev.mysql.com/doc/refman/5.5/en/explain.html). EXPLAIN shows you which columns and which of their indices (if any) are used. It also points out if inefficient strategies, such as "[temporary tables](https://dev.mysql.com/doc/refman/5.7/en/internal-temporary-tables.html)" or "[filesort](https://www.percona.com/blog/2009/03/05/what-does-using-filesort-mean-in-mysql/)" are applied. So: Log your queries, run them through EXPLAIN and build your (multi-column) indexes based on the EXPLAIN results. Most importantly: repeat this from time to time, because things change and different data-size proportions might benefit from different indexes;
* **Avoid JOINs**: JOINs are easily used with modern ORMs and look like a good idea at first glance (and in certain situations they are!). However, when your App grows, they are many times the reason why your database performance decreases hugely. Why? Because there is a major difference between a join between 100 x 100 rows and 10,000 x 100,000 rows: the former fits in the memory, the latter not;
* **Split write-tables & read-tables**: Try to use separate write-intensive and read-intensive tables. Whenever you write on a table, you invalidate the query cache. For instance: if you have a read-intensive user table, and want to log their latest activity, use an additional table for this;


### Static file separation

Sure, a request to a static file can be delivered faster than a request handled by a dynamic script - nonetheless, the more static requests are performed by your App, the more it has to do overall. More is more in the end. If you split up the traffic in dynamic and static requests, by utilizing a cloud hosting service, all the requests for static files are not handled by your App anymore but by another server somewhere else. Resulting, your App on fortrabbit acts as the motor for your PHP scripts, while a host dedicated to delivering static files takes care of the rest.


### Profile your code

Web applications are usually fast after the launch, but degrade over time. They receive more requests and process more data. You add more features and at some point everything or some parts of your app become slow. Profilers give you deep insights of your code and all dependencies that are involved to handle requests. XDEBUG profiler and XHPROF are the defacto open source standard, but you need additional tools to visualize the data. The [blackfire profiler](blackfire) makes it really easy to start profiling in minutes.

### Reduce the max execution time

This is an opinionated topic. If you come from searching 504 time out errors, you will often read to increase the `max_execution_time` setting to allow a certain process to finish it's task. 

By the way: You can adjust this setting with your App under "PHP settings" in the Dashboard. 

In some cases that makes sense, but it's often a poor design decision. Image transformations with imageMagick are often long running, therefore you might want to increase the execution time to avoid time outs. But, as stated here often: long running tasks should best be separated from the frontend tasks into the background. We also have a [Worker service](/worker-pro) for that. 

Frontend requests should always return a swift answer. The number of PHP processes is limited. When all PHP processes are occupied with long running tasks, new requests need to wait. That can easily pile up. A lower max execution time limit often results in faster errors. And that's often what you want. When making an API call to an external service, usually that reply should be there within a second, how long should you wait for it max? Maybe 10 seconds is reasonable. It's not likely that there will be an answer when you wait longer. 

So for most cases, short is better.

## Frontend design

Are you a full-stack developer? So far we have mostly covered performance on the PHP side. A lot of delivery can be optimized in the front facing side of your application. Let's have a look what can be done on the last meters and how to optimize content efficiency:

### Dealing with assets

Assets are static (not dynamically generated) files. JS, CSS, images are the usual suspects here. We recommend to have two versions of your asset files:

* **SRC** - the original files, that you can edit, propbably LESS, SASS, COFFEESCRIPT, included in Git
* **DIST** - the optimized files that are served in production, usually not included in Git

It's handy to automate the process of building the production files with a pipeline tool like [Gulp](http://gulpjs.com/).

#### Images

Images represent around 60% of the total footprint of an average website. Of course you know all about JPG, PNG, SVG and probably WebP. But did you know that there are tools that can optimize your images even further than your image editor (Photoshop)? Have you ever heard about: jpegoptim, jpegtran, jpegtran, OptiPNG, GIFsicle?

There are plugins for your build tool to do that.

#### JS & CSS

Minifying JS/CSS files removes unnecessary extra spaces, line breaks and indentations. Another technique is to combine — concating —  different JS or CSS files into one file. This brings you: less requests and probably better GZIP compression ratios.

Finally, there are specific techniques to further reduce size: For JS, variable names can be changed automatically for shorter ones, in CSS you can use shorthands and short notations of colors and values.

There are plugins for your build tool to do that.

#### Web fonts

Web fonts are nice but they also come with a footprint. The file size is determined by the number of glyphs, metadata and compression. On top of that, there are four different formats and you probably need to include all to have the best coverage.

External services like Google Web Fonts or Typekit can help optimize font size and delivery. Take care to only include the font weights and glyphs you require.

### Caching assets

Don't serve the same content to the same client twice. Cache it with their browser. While modern browsers are already doing great work here. You can further control the caching by altering the Cache-Control with the HTTP headers. See the [.htaccess section](/htaccess#toc-cache-control) for an example.


### GZIP compression with Apache

GZIP provides lossless compression for text files such as HTML, CSS or JS. It's implemented on the web server — Apache in our case otherwise nginx — as a module. You can [enable and configure](quirks#toc-php-compression) it in your `.htaccess` file (see also our [htaccess article](/htaccess)).

It works like this: after all the HTML is rendered on the server side it gets compressed and send to the browser in such a minified format. The browser then has to decompress everything on the fly.

So as you can imagine: that of course saves bandwidth but also costs a little bit of CPU on both sides. It is in general recommended to use it but can cause strange effects when combined with other techniques, like caching.


### Cookies

Eat diet Cookies! Cookies are sent in the HTTP headers in every single request, always. Use a different domain for serving static assets can help to reduce the cookie traffic.

### DNS lookups

You might parallelize downloads across host names. This may help speed up delivery, but if the number of host names is too long the time that the client spends to resolve the domain can affect negatively. The concurrent connections of web browsers is limited.


### Check your speed

Faster websites and apps are more fun to use and are better ranked in search engines. Measure the performance of your App. Master the web developer tools of your browser and use external services such as Google PageSpeed, YSlow and GTmetrix.



## Troubleshooting performance

OK, your App is running, but it "feels slow", you want it faster? Here are the most common mistakes and issues we've encountered.

### External data sources

If you are integrating external services, for example for any kind of REST API, make sure it does not slow you down. Set proper but very small timeouts and log response times once they exceed the threshold. Also make sure you [decouple](#toc-decouple-but-loosely) them.

### Database performance

Our MySQL Add-Ons can scale. Make sure you have the correct size selected. Also very common: check your JOIN queries! If they ran fast when your dataset was small, it doesn't mean they still do! Utilize caches to buffer costly databases results.

### Massive I/O

Heavily used session-files and cache-files can easily slow your App gravely down. Switch to an in-memory cache and/or try to outsource data into your database!

### No cache

If your App runs fast with only a few visitors it doesn't mean it can handle the load once you gain more traction. Cache, then cache some more and finally cache more. It's hard to overstate the importance and the possible speed gain of caching!

## Security design

Last not least: Take care to keep your App secure. Check our [security page](https://www.fortrabbit.com/security) with some best practices and duties.