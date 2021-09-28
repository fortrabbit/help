---

template:         article
reviewed:         2021-09-15
title:            Limitations
naviTitle:        Limits
lead:             Our service comes in different variations and sizes. Here we explain what happens when a limit is reached.
group:            platform
stack:            all

---

We don't believe in hosting offerings with unlimited resources. There is always a limit, even for cloud hosting. Our fair limits are designed to match the most common use cases on this platform. Some limits may seem small, but the vast majority of web projects will not need more. Some of our limits are even intentionally kept small by design to force best practices:

> I just want to say that you all make us better for our clients. The limits you set on the servers are reasonable and prevent us from coming up with lazy or hacked solutions. We are definitely better because of you all.

- Stephen Callender from [Shoe Shine Design](http://shoeshinedesign.com/)

What happens when you hit or even go over one of the scaling limits described in the [specs page](http://www.fortrabbit.com/specs)? All Components are different by nature, the behavior when you exceed a limit is different. The following list helps to understand the consequences and when it is a good idea to scale up or out:

## PHP memory

**Scope**: Universal and Professional Apps

**Description**: The PHP Component on fortrabbit has a memory limit. This limit depends on the selected scaling and is declared on the ([Universal](https://www.fortrabbit.com/specs) & [Professional](https://www.fortrabbit.com/specs-pro#php)) specs pages. Once the memory limit is reached, which can happen due to a [multitude of factors](php-pro#toc-php-memory), the application will start to fail. Failing means either a 503 error or a classic "PHP whitescreen". Depending on the type of failure you can expect to find errors in the log. A typical error would be:

```plain
Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 54 bytes)
```

**Solution**: With Professional Apps, you can solve this issue by upgrading to a larger vertical scaling, which means: If you are using a `PHP s` scaling, upgrade to a `PHP m` scaling with the same amount of nodes. Respectively, if you are using `PHP m`, then upgrade to a corresponding `PHP l` scaling. Each of those upgrades doubles the available memory. Lastly, if `PHP l` is not sufficient, there are dedicated PHP scalings, which offer even more memory, if required.

The current and historic memory consumption of an App can be viewed in the Dashboard (Dashboard > App > See all metrics). The two relevant metrics are _Memory Usage_ and _Swap Usage_. Since both are hourly average values, you will not be able to detect peak usages, but you can monitor trends. To give you an example: A _Memory Usage_ value of 80-90% of the available memory is a soft indicator, that the memory peak usage is probably beyond the available capacity. Now if the _Swap Usage_ value also shows consistently anything beyond 5-10% of the available memory, this soft factor becomes a near certainty.

For high level insight, especially regarding peak consumption, we recommend to use tools like [New Relic](new-relic) or [Blackfire](blackfire).

## PHP processes

**Scope**: Universal and Professional Apps

**Description**: Besides PHP memory, the amount of processes per App are also an important performance metric. See the [specs page for Universal Apps](https://www.fortrabbit.com/specs#plans) for limits. The amount of processes for Professional Apps are [available in the specs](https://www.fortrabbit.com/specs-pro#php).

The amount of processes divided by the average PHP response time determines how many PHP requests the App can handle per second. For example, if the App (on average) responds within 100ms then two processes could handle 20 requests per second:

- `2 processes * 1 second / 100 ms = 20 requests per second`
- `8 processes * 1 second / 150 ms = ~53 requests per second`
- `16 processes * 1 second / 50 ms = 320 requests per second`

If more (PHP handled) requests are coming in than the application can handle, then they will be queued for a short time. If the queue is full, then the request will result in a `504 Gateway Timeout` error.

**Solution**: The Dashboard metric _Page views_ show the amount of PHP requests per hour and the metric _PHP response time_ shows the average response those PHP requests took. With those metrics, you can determine whether your current scaling is sufficient. For example, if you have 100,000 average _Page views_ per hour and those execute on average (_PHP response_time_) in 100 ms, then you know that you need at least three processes to handle it, four to be safer. A simple calculation would be `100,000 requests per hour / 3600 seconds per hour = ~28` then `28 * 0.1 seconds average PHP response time = ~2.8 minimum required processes`.

Mind that those _PHP response time_ values are averages in the hour. If you have a strong difference in response times for individual requests (say 100 ms is the average, while some requests finish in 50 ms and others in 200 ms) then you might need more processes, to compensate for changes in the "request profile" (i.e. if those endpoints which result in 200 ms response time are suddenly more frequently used and your average response time goes toward 200 ms as a result).

Also mind that the hourly _Page view_ metrics do not show you clustering within smaller time periods. If those 100k requests do not happen about evenly distributed within the hour, but in a very short window (say 5 minutes), then you need a lot more processes to handle them properly.

### OPcache

**Scope**: Universal and Professional Apps

The OPcache limit is soft, when exceeded either nothing happens or you see a degraded performance depending on whether the OPcache exhaustion is constantly caused or only in seldom, special requests.

### Pitfall: Self requests

One commonly seen pitfall are so called "self requests". Those are scenarios in which the App executes a PHP script, which fires an HTTP request to the App itself. A simple PHP example would be:

```php
echo file_get_contents('https://{{app-name}}.frb.io/navigation');
```

Now if this script can be queried under the URL `https://{{app-name}}.frb.io/home`, then `/home` makes a self request to `/navigation`.

The danger with using such a technique is producing a [deadlock](https://en.wikipedia.org/wiki/Deadlock): Using the above example imagine an App with two processes on a single node which receives two concurrent requests to `/home`. Both then "internally" generate a new request to `/navigation` and return the output. However, since the App only can handle two concurrent requests with two processes, both of them are locked and wait for the response of the request to `/navigation`. Neither of the requests to `/navigation` can be executed, since both processes are already in use handling the request to `/home`. The App is in a deadlock.

## MySQL storage

**Scope**: Universal and Professional Apps

**Description**: Each MySQL scaling comes with a fixed amount of maximum available MySQL storage - [Universal](http://www.fortrabbit.com/specs#plans) & [Professional](http://www.fortrabbit.com/specs-pro#mysql)). When this limit is exceeded all privileges to write operations, which could create additional data, are suspended. Those write operations are:

- `CREATE TABLE`
- `CREATE VIEW`
- `INSERT`
- `UPDATE`

All other write operations, such as `DELETE` or `DROP`, which are needed for possible cleanup operations, are still allowed. The implementation of those suspensions is time delayed and can take effect a few minutes after the limit has been exceeded.

The MySQL storage limit is critical. When exceeding this, multiple things can happen: maybe nothing happens, or some parts of the site throwing errors or even "white screens". The kind of error depends where write capabilities are used, a news site which can only be written by an editor might deliver just fine - whereas a community site, which stores user comments stops working in large parts.

**Solution**: To re-enable write capabilities you either can upgrade to a bigger scaling or reduce data size by deleting rows or dropping tables. Mind that the time delay goes both ways: When you clean up the DB and remove data size, it can take a couple of minutes for the privileges to become available again.

## MySQL index

**Scope**: Professional Apps

**Description**: Each MySQL scaling comes with a fixed amount of maximum available MySQL index size [Professional](http://www.fortrabbit.com/specs-pro#mysql)). That is just a soft limit. When exceeding, either nothing happens or the App gets slower because the total available dedicated memory is exhausted depending on whether index memory exhaustion is caused permanently or by an isolated event.

## MySQL IOPS

**Scope**: Professional Apps

**Description**: MySQL IOPS is a soft limit. Exceeding it probably degrades performance for website delivery.

## Web storage

**Scope**: Universal Apps

**Description**: Each Universal App comes with a limited amount of available [persistent web storage](app-uni#toc-persistent-storage). The amounts per scaling are available in [our specs](/specs#plans). [Professional Apps](/app-pro) have [ephemeral storage](/app-pro#toc-ephemeral-storage), so uploads will be outsourced to the [Object Storage](/object-storage). Consider that we are doing backups with some plans. Backups have a retention period, so there are multiple backups. For example, when you are using 5 GB web storage, there are 14 copies, so 70 GB in total used. That's really a lot. Each a new copy gets created and transferred. At a certain size and file number those backups might fail.

**Solution**: We currently allow a slight exceeding of those limits, there is no hard capping in place. You might review the files on the web storage (see below), since in our experience there is almost always room for improvement. Or you might upgrade to the next bigger scaling, when available. The storage availability of the biggest Universal App scaling is currently the highest we offer. The purpose of fortrabbit Apps is fast light-weight PHP engines, and the web storage limits are set accordingly. To store many large images or videos, better use an external image/video hosting service or cloud storage. Consider our Professional Stack with Object Storage an alternative.

### Use the disk usage command to get the actual usage

The `du` Terminal command will list you the current size of the `htdocs` folder of your App:

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com du -sh
# You can also use on the App and for specific folders
# You can run the command also when being logged in by SSH
```

### Review files on the web storage for clutter

Login by SSH or SFTP to review your files on your App. In most cases the web storage over-usage is not caused by real world requirements but by misconfiguration. Look out for common issues and unnecessary files filling up space:

- Verbose custom log files
- Not required template fragments
- Custom backups on disk by some CMS plugin
- Large and/or duplicate uploads by CMS editors
- Other general temporary files

### How to clean up web space

- Make sure to have a fresh local backup first
- Delete files by SSH or SFTP

### How to prevent your website to fill up web storage again

- Check and correct CMS or framework config, in particular:
  - Turn off unnecessary backup plugins
  - Turn off verbose logging (fortrabbit should be production)

Especially the backup plugins and settings can be become problematic. Please mind that a backup plugin will not only use disk storage but also CPU power while creating the backups which might reduce web delivery performance. Also mind that such backups are onsite. That means, when your App is not accessible in a case of emergency, your backups will be neither. A solid offsite backup solution is included with some Universal App plans - [see our backups article](/backups)

## Object Storage

**Scope**: Professional Apps with Object Storage Component

**Description**: Each Object Storage scaling comes with a fixed amount of available storage. You can review this amount per scaling on our [specs page](http://www.fortrabbit.com/specs-pro#object-storage). This limit cannot be exceeded. Any write operation (modifying a file or creating a new file) will be rejected, if it would result in exceeding the limit of the scaling.

**Solution**: You can either clean up obsolete data to reduce the used size or upgrade to a bigger scaling. If you need more than our current plans offer, please [get in touch](mailto:support@fortrabbit.com) and let us know how much you need and we will get back to you with an offer.

### Memcache

**Scope**: Professional Apps with Memcache Component

**Description**:  Memcache memory is a soft limit. Exceeding it can cause degraded performance. You might see "empty baskets" or aborted user sessions. See the App metrics in the Dashboard.

### Worker

**Scope**: Professional Apps with Worker Component

**Description**: Worker memory is a soft limit. When exceeding it, either nothing happens or you can see fatal errors or slower execution depending on whether memory exhaustion is caused by permanent usage or isolated events or accidental overlapping of time scheduled jobs. Worker memory is limited in the Dashboard. You can only add as many jobs as included in the selected plan.

## Traffic

**Scope**: Universal and Professional Apps

**Description**: Each App has an included traffic, also known as transfer or bandwidth, amount which can be found in the specs - (see the [Universal specs](http://www.fortrabbit.com/specs#storage-and-traffic) & [Professional specs](http://www.fortrabbit.com/specs-pro#storage-and-traffic)).

**Solution**: Once that limit is exceeded, we will charge per additional 5 GB or part thereof, as described in the above linked specs page. High traffic is often caused by videos or images. Images are often not compressed well enough and videos are big by definition. An alternative way to set delivery of static assets is by using a pull-CDN or by completely outsourcing the static files, either to an image and video hosting service, or to a cloud storage like AWS S3. For the Professional Stack we are offering the Object Storage.
