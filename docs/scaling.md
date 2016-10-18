---

template:      article
reviewed:      2016-06-10
title:         Scaling
naviTitle:     Scaling
lead:          When and how to grow and shrink your Apps resources.
group:         Kitchen_sink

seeAlsoLinks:
    - app-design
    - app
    - terminology

keywords:
    - app-design


---

One of the fortrabbit platform fundamentals is scalability – handle nearly any amount of visitors. Start with low resources and grow wild at any time, increase or decrease computing resources on demand. Each App on fortrabbit has multiple Components which can be scaled individually.

## Understanding vertical & horizontal scaling

```nohighlight
 # Vertical scaling: grow Node
 # Horizontal scaling: add Nodes

 ┌ ─ ─ ─ ─ ─ ─ ┐ ┌ ─ ─ ─ ─ ─ ─ ┐ ┌ ─ ─ ─ ─ ─ ─ ┐ ┌ ─ ─ ─ ─ ─ ─ ┐

 │      ▲      │ │             │ │             │ │             │
        │
 │      │      │ │             │ │             │ │             │
        │
 │      │      │ │             │ │             │ │             │
        │
 │      │      │ │             │ │             │ │             │
        │
 ├──────┼──────┤ │             │ │             │ │             │
 │      │      │
 │      │      │ │             │ │             │ │             │
 │      │      │
 │      └──────┼─┼─────────────┼─┼─────────────┼─┼──────▶      │
 │             │
 │             │ │             │ │             │ │             │
 │             │
 └─────────────┘ └ ─ ─ ─ ─ ─ ─ ┘ └ ─ ─ ─ ─ ─ ─ ┘ └ ─ ─ ─ ─ ─ ─ ┘
```

A Node is a lightweight virtual container that is configured to run a specific service.

### Vertical scaling

Vertical scaling means that you grow your Node size. On fortrabbit this mostly means more memory, but also more [CPU power](http://fortrabbit.com/specs). Which vertical scaling size for which Component your App needs depends in large parts on the technology you are using.

### Horizontal scaling

Horizontal scaling means that you add more Nodes. Once you have figured out which plan size fits your application best, you can just add more Nodes to your App by booking higher plans: Say your App runs on PHP s, then, with increasing visitors, you simply would scale: PHP s 1 -> PHP s 2 -> PHP s 4 and so on.

To make things easier for you, we categorized most Components into the scaling groups (extension levels), which reflect a point in life of your application. It's mostly best practice to combine Components from the same scaling groups, everything on Production level grade, for example, but it depends on the use case.

### Tinkering

Use this in development, as staging environment or for low volume/traffic Apps. This level keeps things simple and affordable by saving on availability and performance.

```
# Tinkering level scaling App example
┌────────┐     ┌────────┐     ┌─────────┐     ┌─────────┐
│Internet├─────▶ Apache ├─────▶ PHP/FPM ├─────▶  MySQL  │
└────────┘     └────────┘     └─────────┘     └─────────┘
```

### Production

Use for live web applications, for which performance and high availability is key.

```
# Production & Dedicated level scaling App
               ┌──────────┐   ┌────────┐   ┌─────────┐   ┌──────────────────┐
               │          ├───▶ Apache ├───▶ PHP/FPM ├───▶  MySQL Cluster   │┐ <------`
               │          ││  └────────┘   └─────────┘   └──────────────────┘│        |
┌────────┐     │   Load   ││                    │         └───── ▲ ──────────┘        |
│Internet├─────▶ Balancer ││                    ├────────────────┤                  Worker
└────────┘     │ Cluster  ││                    │                ▼                    |
               │          ││  ┌────────┐   ┌─────────┐   ┌──────────────────┐         |
               │          ├───▶ Apache ├───▶ PHP/FPM ├───▶ Memcache Cluster │┐<-------`
               └──────────┘│  └────────┘   └─────────┘   └──────────────────┘│
                 └─────────┘                              └──────────────────┘
```

### Dedicated

For those lucky few wildly successful web applications. Dedicated private cloud resources give you more control and of course ultimate power.


### When to grow horizontally

There are three common scenarios which require a scalable, elastic, environment:

**Continuous growth**: Your business grows constantly. You should be able to anticipate the acceleration of the growth because you are planning for it and actively take actions to increase and guide it.

**Event based**: You might have an e-shop, and the Christmas sales are coming. Or you promote a festival, which is once or twice a year. Those events are strongly connected to your business and you plan on them.

**Slashdot effect**: An extraordinary event occurs — you've been mentioned on a major news site (hackernews, product hunt …) or a television channel is reporting about you. In short: you haven't planned for this.

Our scaling capabilities are focused on the first two scenarios, because the slashdot events are quite rare and - well - unforeseeable by nature. There is no auto-scaling in place, but you can easily and quickly react on traffic peaks by logging into the Dashboard and scaling the App — scaling happens almost immediately.

### Comparing with metrics

The fortrabbit [Dashboard](/dashboard) provides you with useful metrics on how your App is doing, what it is handling and how much memory it consumes. This can help you fine tune for the best scaling settings.


## How to scale

That's the easiest part: login to the [Dashboard](/dashboard), go to your App, click on the Component you want to scale and choose a new scaling. Scaling happens almost instantly and can easily be reversed. The minimum billing period of one day makes it easy to experiment with different settings. Try out a higher scaling level for a day, and you'll only be charged for one day. Scaling happens within minutes, in most cases without downtime.

## What to scale

So what is there to scale? Given a typical PHP web application, you have these main resources (we call them Components) to scale individually:

### PHP

PHP is the main Component on fortrabbit, you can scale it vertically and horizontally. Please see our dedicated [article on scaling PHP](php-scaling).


### MySQL

While PHP implements the logic, the [MySQL database](mysql) (most of the time) contains the data the logic works on. Each database has physical limits (disk performance, amount of memory, available CPUs).

You should choose a MySQL plan which has at least as many connections as your PHP plan offers processes. An overview of processes per PHP plan and connections per MySQL plan can be found in our [specs table](http://www.fortrabbit.com/specs).

All Production and Dedicated MySQL scaling are replicated. This means: they run on two MySQL Nodes at the same time. One of those Nodes is called "master", the other "slave". The master Node is the active Nodes, to which your App automatically connects to. If the master fails then the slave takes automatically over and your App keeps running.

Also see the quirk when [scaling MySQL with persistent connections](quirks#toc-mysql-with-persistent-connections-during-upgrade).


### Memcache

[Memcache](/memcache) is a Component which can be booked optionally. Memcache is recommended in addition to all PHP production scalings to store session data across Nodes.

Production level Memcache scalings run on two Nodes. Now it depends on how you use/configure your Memcache client. You can either use both Nodes as a single, joined space, which gives you double the memory, or you can use them redundantly, which leaves you with half of that. Only the latter case will provide high availability, since it allows the failure of one of the two Nodes. See also the main [Memcache article](/memcache).

### Workers

[Workers](/workers) help to offload compute intensive tasks, they can be booked optionally.

All worker plans run only on a single Node. In case one of those Nodes fails they are restarted automatically. This does not guarantee high availability but eventual availability, which is in almost all cases a good approach for executing background tasks. See also the main [Workers article](/workers).


### Object Storage

[fortrabbit (New) Apps](new-apps) have an ephemeral local storage, which means that all files on the server will be wiped on each deploy or change of configuration, also on maintenance. 

Content driven websites usually make use of the local file system to store assets. If you have a "media website" or an social community focused on image or videos for example, the storage needs to be able to scale as well. For those use cases we recommend to use our [Object Storage](object-storage) component. It's easy to setup, supports best practices and reduces load on the PHP Nodes.

The Object Storage plans are limited by storage (not traffic). It will not scale automatically, you need to keep an eye on the size. In the App overview in the Dashboard you can find a graph that shows you home much storage within the current plan is in use. Scaling up and down can (almost) instantly be done in the Dashboard.


## Clean code safes money

We are in this together. The smarter you code, the less resources you'll need. Or to put it in more dramatic words: bad code doesn't scale. So please don't miss our [application design and coding practices article](app-design).

## Advanced specifications

Our [specs page](http://www.fortrabbit.com/specs) provides you with fine-grained informations about plans and limits.

## Exceeding limits

What happens when you hit or even go over one of the scaling limits described in the [specs page](http://www.fortrabbit.com/specs)? As described above, there is no auto-scaling in place. As all Components are different by nature, the behavior when you exceed a limit is different. The following list helps to understanding the consequences and when it is a good idea to scale up or out:


### App base limits

**Traffic** is measured with the App. When an App exceeds the included traffic, additional traffic will be billed. See the [specs page](http://www.fortrabbit.com/specs) for costs and packaged. 
s

### PHP plan limits

**PHP memory** is a soft limit, when an App exceeds this limit either nothing happens or you can see a degraded App performance or even "white screens". This depends on whether the memory exhaustion is constantly or only in isolated events and whether the additional memory is swappable. See the PHP memory consumption with your Apps metrics in the Dashboard.

**PHP requests** limits shown in the specs page are only a recommendation. When exceeded for the chosen PHP plan the App might responds slower.

**OPcache** is a soft limit, when exeeded either nothing happens or you see a degraded performance depending on whether the OPcache exhaustion is constantly caused or only in seldom, special requests.


### MySQL limits

**MySQL storage** limit is critical. When exceeding: either nothing happens, or some parts of the site throw errors or a "white screen". This depends where write capabilities are used (eg news site which can only be written by admin works fine - whereas community site, which stores user comments stops working in large parts).

**MySQL memory** limit is only a recommendation. As long as: `memory + index < total storage` all is good.

**MySQL index** limit on tinkering & production plans is only a recommendation. See above.

**MySQL index** limit on a dedicated plan is a soft limit. When exceeding, either nothing happens or the App get's slower because the total available dedicated memory is exhausted depending on whether index memory exhaustion is caused permanently or by isolated event.

**MySQL IOPS** limit is a soft limit. Exceeding it probably degrades performance for website delivery.


### Memcache limits

**Memcache memory** is a soft limit. Exceeding it can cause degraded performance. You might see "empty baskets" or aborted user sessions. See the App metrics in the Dashboard.


### Worker limits

**Worker memory** is a soft limit. When exceeding it, either nothing happens or you can see fatal errors or slower execution depending on whether memory exhaustion is caused by permanent usage or isolated events or accidental overlapping of time scheduled jobs.

**Worker memory** is limited in the Dashboard. You can only add a s many jobs as included in the selected plan.

<!--  TODO: object storage launch -->
