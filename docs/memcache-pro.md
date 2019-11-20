---

template:    article
reviewed:    2019-09-30
title:       All about Memcache
naviTitle:   Memcache
lead:        Speed speed speed. Why and how to do caching with Memcache on fortrabbit.
group:       Components
stack:       pro

keywords:
   - memcached
   - memcache
   - cache

---


## Problem

[Caching](best-practices#toc-prepare-to-cache) is a cornerstone of the "snappy App". As soon as your App spreads across more than one PHP Node you probably need a network cache. Otherwise, user sessions would not work.

## Solution

Meet Memcache(d) caching: the standard network cache which is extremely fast and reliable. Applying it correctly can boost the performance of most web applications dramatically. We offer optional Memcache scalings, which provision dedicated memcached servers, running on a single or two Nodes for data redundancy.

The foremost use-case example are user sessions in the aforementioned multi-node scenario. Aside from those any kind of buffered data - such as expensive MySQL query results - can be stored and received extremely fast.


## Usage

You can enable and scale Memcache in the [Dashboard](dashboard). The smallest available scaling is a memcached server running on a single Node and is primarily recommended for development scenarios. The additional Production scalings grow in size of memory and span across two nodes to offer high availability in outage scenarios.

In PHP you have two drivers to connect to a memcached server: [Memcache](http://php.net/manual/en/book.memcache.php) and [Memcached](http://php.net/manual/en/book.memcached.php). If in doubt, use the former one.

## Production PHP and Memcache

All PHP production scaling are running on two Nodes. If your App needs to handle session data, you need to keep it consistent across the Nodes. Memcache is also our recommended a solution for this.

## Access Memcache from your App

```php
// Enable redundant mode when using the `Memcached` library
$secrets = json_decode(file_get_contents($_SERVER["APP_SECRETS"]), true);
$mc      = $secrets['MEMCACHE'];

// enable redundant session handler
ini_set('session.save_handler', 'memcached');
ini_set('memcached.sess_number_of_replicas', 1);
ini_set('memcached.sess_consistent_hash', 1);
ini_set('memcached.sess_binary', 1);
ini_set('session.save_path', implode(", ", [
    "{$mc['HOST1']}:{$mc['PORT1']}",
    "{$mc['HOST2']}:{$mc['PORT2']}"
]));

session_start();

// use redundant memcache for user data
$m = new Memcached();
$m->addServer($mc['HOST1'], $mc['PORT1']);
$m->addServer($mc['HOST2'], $mc['PORT2']);
$m->set('SomeKey', 123);
$m->get('SomeKey');
```

See also the specific examples for: [Laravel](install-laravel#toc-memcache), [Symfony](install-symfony#toc-memcache), [Craft](install-craft#toc-memcache).



## Scaling

In the Dashboard, go to your App and click on Memcache under the scaling options. Please also see the [Memcache scaling](scaling#toc-memcache) section. Using a plan with two Nodes allow you two modes:

1. **Combined**: data is stored only on one of the nodes > twice the memory size
2. **Redundant** (recommended): data is stored on both nodes > one node can fail


### Redundant setup

The PHP Memcached extension can be heavily configured and tuned. Individual App requirements might result very custom settings. With that in mind, following our own best practice advice for a redundant setup:

``` php
// Initialize Memcached with an ID, which allows persistent connections
$id = getenv("APP_NAME");
$mc = new \Memcached($id);

// Use a global, tunable timeout, from which all time-related tuning
// options derive
$timeout = 50;

// Set options
$mc->setOptions([

    // Assure that dead servers are properly removed and ...
    \Memcached::OPT_REMOVE_FAILED_SERVERS => true,

    // ... retried after a short while (here: 2 seconds)
    \Memcached::OPT_RETRY_TIMEOUT         => 2,

    // KETAMA must be enabled so that replication can be used
    \Memcached::OPT_LIBKETAMA_COMPATIBLE  => true,

    // Replicate the data, i.e. write it to both memcached servers
    \Memcached::OPT_NUMBER_OF_REPLICAS    => 1,

    // Those values assure that a dead (due to increased latency or
    // really unresponsive) memcached server increased dropped fast
    // and the other is used.
    \Memcached::OPT_POLL_TIMEOUT          => $timeout,           // milliseconds
    \Memcached::OPT_SEND_TIMEOUT          => $timeout * 1000,    // microseconds
    \Memcached::OPT_RECV_TIMEOUT          => $timeout * 1000,    // microseconds
    \Memcached::OPT_CONNECT_TIMEOUT       => $timeout,           // milliseconds

    // Further performance tuning
    \Memcached::OPT_BINARY_PROTOCOL       => true,
    \Memcached::OPT_NO_BLOCK              => true,
]);

// Init servers only if they are not already added (using above $id
// results in a persistent setup, which keeps the connection in between
// handling (requests), so the servers need only be added in the "first
// handling")
if (!$mc->getServerList()) {
    foreach ($servers as $server) {
        $mc->addServer($server[0], $server[1]);
    }
}
```

### Pitfall: Memcached::getVersion vs redundant setup

Some very popular Memcached adapters use the `getVersion` method of the Memcached extension to check the state of the connection after setup. We do not recommend this, [due to incompatibility of this approach with redundant setups](https://github.com/laravel/framework/issues/17957).

## Alternatives

There are some other cache stores, most importantly [Redis](http://redis.io/). We currently don't offer any of those directly, but you can easily use a 3rd party provider with your fortrabbit App.
