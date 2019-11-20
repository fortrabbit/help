---

template:     article
reviewed:     2019-10-28
title:        Worker
naviTitle:    Worker
lead:         Offshore long running and compute intensive tasks with background jobs — that's what the Worker Component is for.
group:        Components
stack:        pro
uniLink:      cron-job-uni

---

<!--

TODO: say something about message queuing ??

-->

## Problem

Simple truth: websites are better when faster. Don't let your users suffer when your App is busy: generating caches, sending transactional mails, communicating with remote APIs, RSS feed reading and parsing, image processing, uploading data to external storage — these tasks are taking long to execute and slow down your App.


## Solution

```nohighlight
|||||||||||                        ┌───────┐      ┌────────┐
│         │                        │       │      │        │
│ ◎    ◎  │                        │       │      │        │
│    |    │ ◀─ Request/response ─▶ │  App  │ ◀──▶ │ Worker │
│  ~  ~   │                        │       │      │        │
│ Visitor │                        │       │      │        │
└─────────┘                        └───────┘      └────────┘
```

What you want is: dedicated back-end PHP processes that are totally isolated from your web front-end. Use the Worker Component to offload those long running tasks. Background jobs are a key to resilient web applications. They transfer time and compute intensive tasks from the front-end web layer to a background process that lives outside the user request/response life-cycle.

**Rule of thumb**: Consider using a background job for any web request that runs longer than 500ms.



## Booking & scaling

The Worker is an optional Component that can be booked and scaled from the [Apps](app) overview in the [Dashboard](dashboard). Workers, like most other Components, are available in various extension states. The scaling is linear and increases two parameters:

* available RAM, we assume a minimum of 128 MB per job
* number of enabled jobs

In addition the CPU resources available scale linear with the amount of memory: a plan with 512 MB memory has access to four times the CPU time as a plan with 128 MB memory.

The memory limit is for all jobs combined. For *Worker L* plans and above, there's also a default soft limit of 1024 MB memory per job to ensure there's enough for all concurrently running jobs - this can be overwritten with `ini_set('memory_limit', xxxxM');` up to your Worker plan limit. The number of configurable jobs is based on the average usage we have seen on our platform. Resources needed for each application can vary largely, depending on what each particular job is doing. It's not unheard of for a single job to consume beyond 512 MB when doing extremely memory intensive tasks. In this case you need to select a plan accordingly.

* See the [specs page](http://www.fortrabbit.com/specs#worker) for limits.

Please mind that the Worker Component is not available for Apps in trail mode.

## Using the Worker

Once you have booked the Worker Component, an additional setting page is available from your Apps overview in the Dashboard. Here you can add, remove, start, stop and edit jobs. A Worker performs two kind of jobs:


### 1. Nonstop Jobs

Nonstop Jobs are continuous running PHP processes. They are meant to run forever and will be automatically restarted if they fail. Most Nonstop Job solutions use queues to inject tasks from the web application into the Nonstop Job (running in the background).

**Example — Transforming images**: A visitor uploads an image to the web application. Instead of transforming the image directly, which would slow down the web application, because it costs lots of compute power and can take a long time, the web application creates a new task in a queue, which is very inexpensive. The running Nonstop Job then receives the task from the queue and transforms the image. Since the transformation runs in the background it does not matter (nearly as much) how long it takes - eventually it will finish and the web application stays fast and responsive.

**Example — Sending e-mails**: A visitor signs up to the web application. After this the user shall receive a welcome e-mail. Since the used e-mail transport can be temporarily down, or busy and slow responding, the web application creates a new e-mail task in a queue instead. The Nonstop Job then receives the task from the queue and sends the e-mail out. If the e-mail service is currently very busy and slow it doesn't matter: the web applications stays superbly fast and responsive and the mails are sent eventually from the Nonstop Job.

#### Dashboard configurations

* **Name**: A unique name, identifier for Dashboard, logs & statistics
* **Command**: PHP command to be executed, for example `artisan queue:listen -v` or `path/to/my-script.php`
* **Termination Signal**: Unix termination signal to restart job, just use default when unsure
* **Termination Timeout**: Grace time after which the job will be "hard killed" (`SIGKILL`), if shutdown termination signal has been sent
* **Status**: You can temporary disable jobs (start/stop)



### 2. Cron Jobs

Cron Jobs are time scheduled PHP executions. They run at defined times, independent of visits to the web application.

**Example: Database maintenance**: say the web application cumulates data which needs to be transformed and/or wiped periodically. A Cron Job allows you to make sure the `app/console db:cleanup` - or whatever - script executes hourly, daily, weekly or whenever your want.

**Example: Cache clearing**: say the web application has a news site, which homepage must be rebuilt every ten minutes or so. With a Cron Job you can schedule a cleanup of the homepage every one, ten, thirty or whatever minutes required.

#### Dashboard configurations

* **Name**: A unique name, so the job can be identified later on in the logs or statistics.
* **Command**: The PHP command which shall be executed, eg `app/console db:cleanup` or `path/to/my-script.php`
* **Interval**: The interval at which you want to execute the job.
* **Status**: You can temporary disable jobs

#### Intervals

1. every minute
2. every 10 minutes
2. every 30 minutes
3. every hour
4. every day
5. every week
6. every month

The interval timing is guaranteed, the exact time of execution is randomized. For example: 30 minutes will run at every 13th minute and at the 43rd minute again. All daily, weekly and monthly jobs run between 00:00 and 10:00 UTC. Weekly intervals will run on Monday.


### Metrics

There are two ways to monitor your worker: statistics (see [below](#toc-job-statistics)) via SSH and metrics within the App metrics overview in the Dashboard. The last will inform you about:

**Swap Usage**: The amount of swapped memory your Worker jobs are causing. Any swap greater than zero indicates that you should upgrade your Worker component

**Memory Usage**: The amount of memory your Worker jobs are using. Can be higher than the total sum of memory of all jobs, because it includes Linux VFS cache. Primarily interesting in terms of change over time: is there something growing out of bounds?

**Cron Job runs**: Total amount of Cron Job executions over time.

**Cron Job fails**: Total amount of failed Cron Jobs over time. Any value greater than zero is a good reason to look into the Worker logs.

**Nonstop (re)starts**: Total amount of (re)starts of Nonstop Jobs. Mind that each git push triggers a restart. If restarts occur outside of deployment, then the Nonstop Jobs have been dying and were restarted. A good cause to look into the logs.



## Accessing job output

Both STDOUT and STDERR generated by any job are [logged](/logging-pro):

```bash
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:worker

# STDOUT from the the job "a-job" - generated via `echo "The message\n"`
# 2019-01-14T12:00:01Z INFO (a-job): The message

# STDERR from the job "a-job" - generated via `error_log("The message")`
# 2019-01-14T12:00:02Z ERR  (a-job): The message
```

First comes the formatted time ([RFC 3339](https://tools.ietf.org/html/rfc3339)), followed by the log level, the name of the job and finally the message which has been written.

## Restart job after code update

This happens automatically. Whenever you push a new code update via Git all Nonstop and Cron Jobs will be shutdown and started anew.


### Job statistics

Statistics about Nonstop and Cron Jobs help to estimate which Worker plan is the right one and to become aware of possible problems or resource shortages. Get them in your terminal:

```
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com jobs

#  Name               │ Duration      │ Memory                │ Fails  │ Exits (OK)    │ Running
# ────────────────────┼───────────────┼───────────────────────┼────────┼───────────────┼─────────────────
#  a-cron             │ 5s (avg)      │ 15.1 MB (avg)         │ 2      │ 44            │ -
#                     │ 5s (max)      │ 15.1 MB (max)         │        │               │
# ────────────────────┼───────────────┼───────────────────────┼────────┼───────────────┼─────────────────
#  a-worker           │ -             │ 10.7 MB (avg)         │ 0      │ 0             │ true
#                     │               │ 18.0 MB (max)         │        │               │
```

The columns are:

* **Name**: The name of the job which has been setup in the Dashboard
* **Duration**: Only Cron Jobs, average & max duration of execution
* **Memory**: Average & max memory consumption, includes all possibly spawned (forked) child processes.
* **Fails**: Counter how often this Job has exited with a non-zero return code (eg `exit(1)` or `die("foo")` or `throw new \RuntimeException("bar")`..)
* **Exits (OK)**: Successful executions for Crons. Nonstop Jobs should usually not exit, but if they do this counts how often they did.
* **Running**: Whether Nonstop Job is currently running.


### Resetting the counter

To start fresh you can reset the Fail and Exit counters using the `jobs_reset` command:

``` bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com jobs_reset
# This just resets not the jobs itself
```


### Statistic considerations

* The total (max) memory amount of all Nonstop Jobs should be below the memory limit of the plan.
* Cron Jobs need memory as well: If the Nonstop Jobs already consume most of the memory, the Crons will probably fail.
* Cron Jobs can overlap. Make sure that they have all the memory if they do.
* A high Fails count is definitely a problem you should look into. Reset the counters (see above) once the problem is solved.
* Exits of Nonstop Jobs should be looked into. Usually Nonstop run continuously.

## Graceful shutdown

Say your Nonstop Job does really long running stuff or is very busy - meaning that it's likely that it is currently working when you push new code, which leads to a Restart of the job. In this case, you might not want that the job is aborted (restarted) while it's running. The solution is to utilize Unix signal handling to write a shutdown handler. For this you can use the automatically available [PCNTL](http://php.net/manual/en/book.pcntl.php) extension.

**Example**

The most simplistic PHP script for a Nonstop Job is a while loop:

```php
while (true) {
    do_something();
    sleep(5);
}
```

To make sure that `do_something()` is never aborted, you can extend the script like so:

```php
declare(ticks=1);

$shutdown = false;
pcntl_signal(SIGTERM, function($signo) use (&$shutdown) {
    error_log("Received shutdown signal");
    $shutdown = true;
});

while (true) {
    do_something();
    foreach (range(1, 5) as $num) {
        if ($shutdown) {
            error_log("Shutting down safely");
            exit(0);
        }
        sleep(1);
    }
}
```


## Do not detach

If you don't know what that is: never mind. If you do know: don't detach. To guarantee that we can monitor jobs correctly they need to run with-under the parent processes which started them. All detached processes will be killed.


## Developing your worker

When developing you Worker script/command it helps if you are able to execute it once and see what it does. To that end you can use [remote SSH commands](/remote-ssh-execution-pro).


## Alternatives to the Worker

Sometimes you might just want to run a small not compute intensive script. So the above described solutions might be a bit over-sized. Maybe just use an external cron job service for this.
