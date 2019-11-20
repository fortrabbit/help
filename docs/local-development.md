---

template:     article
reviewed:     2019-09-30
title:        Local development
naviTitle:    Local development
lead:         Why and how to set up a local PHP development environment.
group:        deployment
stack:        all

keywords:
     - LAMP
     - vhosts
     - localhost
     - 127.0.0.1

---

## Problem

fortrabbit [Apps](app) are made for production (live) or staging (review). They are not well suited for online-only development. The deployment is fast but still takes too long when developing rapid changing aspects of your App. Also, it is never a good idea to have your live application display errors and debugging informations.


## Solution

Set up a local PHP development environment so that your Apps can run on your local machine. Developing locally is by far the fastest way to see changes quickly. Browse and test your websites and web applications in the browser before deploying them. Also, this way, you always have an up-to-date backup of your code. In other words: A local PHP development environment is key for a successful App lifetime management and continuous deployment.

### Required software

The following open source software should run in your local development environment:

* **Apache** or NGNIX — a web server
* **MySQL** – the database - [see dedicated article](/mysql) on remote usage
* **PHP** – the server side scripting language
* **Git** – the version control - [see dedicated article](/git)
* **Composer** – the dependency manager for PHP - [see dedicated section](/composer#toc-local-composer)

#### Additionally recommended

* **Node.js** and NPM for frontend build processes

## Setup

There are multiple ways to get your local development stack up and running. Choose the one that best fits your skills and needs. Also, your Operating System (Windows, macOS or Linux) might affect your choice here:

### A local PHP server

You can run a web server and PHP directly on your local machine. This is probably the quickest and easiest way to get up and running. Your options:

#### GUIs

These solution stacks are easy to handle through a graphical interface and they interfere with the rest of your system as little as possible. The most commonly used here are:

* [MAMP](https://www.mamp.info/) GUI for Mac OS and Windows (free and paid version)
* [XAAMP](https://www.apachefriends.org/index.html) GUI for Windows, Mac OS, Linux

Please mind that those don't include [Git](git) and [Composer](composer).

#### Laravel Valet

This is an really easy-to-use local solution <del>just for</del> **not only** for Laravel developers on macOS. You have to install it with the Terminal. It's best installed with Homebrew and it requires Composer. The setup is easier than you think and it aligns with best practices of modern development. It bundles NGNIX, DnsMasq and some other magic to an easy to use CLI. 

* [Offcial docs to install Laravel Valet](https://laravel.com/docs/valet)



#### Manual setup

You can also install and manage the software to run your local web server yourself. You'll find lot's of tutorials when Googling around.

### Virtualization

Utilizing a virtualization for development allows you better replication of your local setup across your team. Also having everything stashed into a virtual machine very much nullifies any interference with your local host system. It keeps things clean. There are two primary virtualization approaches for development:

#### Desktop virtualization

[Desktop virtualization](https://en.wikipedia.org/wiki/Desktop_virtualization) is the classic way, it's likely easier to setup for novice users, but eats up more resources. Example VM applications are: [VirtualBox](https://www.virtualbox.org/)(free by Oracle) or [VMware](http://www.vmware.com/)(paid).

VirtualBox in combination controlled by [Vagrant](https://www.vagrantup.com/) helps a lot when setting up reproducible development environments from scratch - or rather from config file. Many Laravel developers use [Homestead](https://laravel.com/docs/5.2/homestead), which builds upon Vagrant and simplifies Laravel development even further.

#### Container virtualization

[Container virtualization](https://en.wikipedia.org/wiki/Operating-system-level_virtualization) is harder to setup, but once done easier to use and is very economical in terms of resource usage. [Docker](http://www.docker.org/) is the goto tool. Have a look at [Docker for Mac](https://docs.docker.com/docker-for-mac/) or [for Windows](https://docs.docker.com/docker-for-windows/) on how to install and get started.

## Considerations

What else you need to think about when setting up your local development environment?

### Virtual hosts

By default your local development environment will likely be reached under: `localhost` or `127.0.0.1`. You might set up multiple vhosts to serve multiple websites from one machine at the same time. Our workflow is to run the App locally under `{{app-name}}.test`, while it runs under `{{app-name}}.frb.io` on fortrabbit.

### Environment detection

<!-- TODO: write more here. This is linked from craft and currently is a bit of a dead end without much info. -->

Our code examples in the [install guides](/#install-guides) always include checks to detect if the App is running locally or on fortrabbit. You should consider this when developing your code - or rather setting up your configuration. For example: MySQL will need different credentials locally then on fortrabbit.

### Multi-staging

You might want to have a dedicated remote testing environment with public access? Head over to our advanced [multi-staging article](multi-staging).

### Apache instead of NGINX

Some might prefer a NGINX web-server over Apache as it is easier to configure. Mind that fortrabbit is using Apache, some features might not be compatible. You will find `.htaccess` examples in these articles which will only work with Apache.