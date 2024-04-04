---

template:         article
naviTitle:        Redis Labs
reviewed:         2023-11-15 15:55:25
title:            Using Redis Labs with fortrabbit
group:            Databases
section:          Extending_fortrabbit
stack:            all

dontList:         true
deprecated:       false

websiteLink:      https://redislabs.com/
websiteLinkText:  redislabs.com
dataCenters:      US, EU
image:            redis-cloud-logo.png

---

## About Redis

Redis is an open source, in-memory data structure store, used as a database, cache or queue message broker.

## About Redis Labs

Redis Labs is a popular hosted Redis provider. They also support the open-source project. Redis Labs offer a public cloud model: it's called "Redis Cloud" and brings "Fully-Managed Redis-as-a-Service".

## Pricing

Redis Labs starts with a free plan which you can scale vertically by size (amount of available memory) and horizontally (going highly available with multi-az). See the [Redis Labs pricing page](https://redislabs.com/redis-enterprise-cloud/essentials-pricing/).

## Signing Up

The sign up is very simple and only takes a minute. All you need to do is fill in your name, email and choose a new password. Then click on the confirmation mail and you are in.

## Booking

Now you need to choose the Cloud (aka data center location) where your Redis instance will run. The best location depends on where your fortrabbit App runs:

* Europe: Choose `AWS/eu-west-1`
* USA: Choose `AWS/us-east-1`

Now you need to decide which availability level you want to subscribe to: "Standard" is cheaper and "Multi-AZ" provides higher availability. So if you are using mainly Production level plans on fortrabbit your best choice is "Multi-AZ". Size the plan according to your requirements. Since you can upgrade later we recommend starting small.

In the next step you can name your resource (aka Redis server). Our recommended naming scheme is `frbit-your-app-name`. Also set up a good password, which, of course, should differ from your login password.

## Connecting

When your subscription becomes available (you can see the status in the Redis Labs control panel) you will get an "Endpoint", which consists of a hostname and a port. For example: `pub-redis-12345.us-east-1-3.7.ec2.redislabs.com:12345`. The first part (`pub-redis...redislabs.com`) is the hostname and the last part (`12345`) is the port.

We recommend to store this information in your App's [secrets](secrets). Go to the fortrabbit Dashboard > Your App > Settings > App Secrets and add:

```plain
# the hostname and port
REDIS_HOST=pub-redis-12345.us-east-1-3.7.ec2.redislabs.com
REDIS_PORT=12345

# the password you chose
REDIS_PASSWORD=your-password
```

To use Redis Cloud from your fortrabbit App you need to do two more things:

### 1. Request a firewall whitelisting

By default all outgoing calls to non-standard ports from your fortrabbit App are blocked for [security](security) reasons. But you can request the fortrabbit team to open up any port for you. That doesn't take long and isn't complicated.

Login to the fortrabbit Dashboard, navigate to your App > Settings > Firewall whitelist and request a custom firewall rule. Write nothing under the optional IP field and insert the port you got from Redis Labs in the Port field. For the description field we suggest "Redis Labs" or the like. Once your request has been approved, which usually doesn't take very long, you are ready to use your new Redis database!

### 2. Enable the PHP (phpredis) extension

While you are logged in the Dashboard, navigate to your App > Settings > PHP and enable the `redis` extension.

## Using Redis in your application

Redis is supported by many PHP frameworks and CMS's out of the box. 

For specific integrations check out the [install guides](/#install-guides) and find out how to use it with your favorite framework or CMS. We recommend using persistent connections. Most adapters are configurable with a setting like `persistent: 1`.

## Session handler

If you don't use a framework, configure `session.save_handler` and `session.save_path` in your php.ini to tell phpredis where to store the sessions. Make sure to do it very early in your application, before accessing session data.

```php
// Read the secrets you've set in the Dashboard
$secrets = json_decode(file_get_contents($_SERVER["APP_SECRETS"]), true);
$host    = $secrets['CUSTOM']['REDIS_HOST'];
$port    = $secrets['CUSTOM']['REDIS_PORT'];
$auth    = $secrets['CUSTOM']['REDIS_PASSWORD'];

// Change the session handler
ini_set('session.save_handler', 'redis');
ini_set("session.save_path", "tcp://{$host}:{$port}?persistent=1&timeout=2&auth={$auth}"); 
```
