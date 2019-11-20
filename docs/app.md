---

template:      article
reviewed:      2019-10-28
naviTitle:     About Apps
title:         What is an App?
lead:          Forget servers. Think services instead. Learn the basic fortrabbit concepts.
group:         platform
stack:         all

---

## Understanding the platform

We claim that the fortrabbit hosting platform is different to classical hosting. Our comparison pages give you a good picture what fortrabbit is about:

* [How fortrabbit is different than VPS hosting](https://www.fortrabbit.com/why-not-vps)
* [How IaaS is being used](https://www.fortrabbit.com/why-not-aws)
* [See the fortrabbit terminology](/terminology)



## The App Concept

```nohighlight
# simplified App topology
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃App                                                 ┃
┃  ┏━━━━━━━━━━━━━━━┓ ┏━━━━━━━━━━━━━┓ ┏━━━━━━━━━━━━━┓ ┃
┃  ┃               ┃ ┃Webserver    ┃ ┃Database     ┃ ┃
┃  ┃               ┣─┫& PHP        ┣─┫cluster      ┃ ┃
┃  ┃               ┃ ┃             ┃ ┃             ┃ ┃
┃  ┃Load Balancer  ┃ ┗━━━━━━┳━━━━━━┛ ┗━━━━━━┳━━━━━━┛ ┃
┃  ┃               ┃ ┏━━━━━━┻━━━━━━┓ ┏━━━━━━┻━━━━━━┓ ┃
┃  ┃               ┃ ┃Webserver    ┃ ┃Database     ┃ ┃
┃  ┃               ┣─┫& PHP        ┣─┫cluster      ┃ ┃
┃  ┃               ┃ ┃             ┃ ┃             ┃ ┃
┃  ┗━━━━━━━━━━━━━━━┛ ┗━━━━━━━━━━━━━┛ ┗━━━━━━━━━━━━━┛ ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

An App is a virtual container for your web project, website, web application, staging branch, project or whatever you do.

### Multiple Apps per Company

fortrabbit hosting has a multi-seat and multi-tenant model. Apps are owned by [Companies](/company) (your business) and managed by [Accounts](/account) (you). The App plans displayed on the [pricing page](https://fortrabbit.com/pricing) are per App. You can create as many Apps as you want of course.

### One website per App

With classical server hosting you rent out a machine to host multiple websites. While this seems to make economic sense — it's not smart. Just think about these scenarios:

* "website A" needs runtime version y, while "website B" requires an version x
* "website C" has a nasty bug and crashes the whole server
* "website D" has a security vulnerability, the whole server get's hacked
* "website E" get's popular and receives lot's of traffic, slowing other websites down
* Your fellow web developer colleague "accidentally" removed all files on the server

Apps are better:

* Apps start a friction of a server but can be scaled across multiple servers
* Each App has its own settings
* Apps are isolated from each other


So, a fortrabbit App is designed to host one website:

* There is only one MySQL database per App
* Each App has only one Git repo
* Each App has its own collaboration rules
* Each App has its own performance metrics

<div markdown="1" data-user="known">

## Dashboard links

* [Go to your list of Apps](https://dashboard.fortrabbit.com/apps)
* [Go to your App: {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}})

</div>


## Two App types

fortrabbit comes in two flavors (stacks): Universal & Professional. With each App you create, you can choose between either one. This can't be changed later on. The two Stacks are different from each other. The rule of thumb is: When unsure, choose Universal. Learn more here:

* [About the Stacks](stacks)
* [About Universal Apps](app-uni) and what is unique about them
* [About Professional Apps](app-pro) and what is unique about them

The following sections here are describing functionalities that are shared by the two different stacks.

## Creating an App

When you sign up to fortrabbit, a new [trial App](#toc-the-app-trial) will be created on the fly. Later on you can create any number Apps in the Dashboard —  just find an accordingly named button. While creating an App, there are a few things to decide: 

1. **App Name**: see [below](#toc-app-name)
2. **Software Preset:** see [below](#toc-software-preset)
3. **Data center location:** Where your App shall be hosted?
4. **Stack & scaling**: [Uni](app-uni) or [Pro](app-pro), plan, or trial?

### Software Preset

While creating an App you can choose from a variety of popular open source PHP software types. This will **not install** the software on your behalf, but prepare some settings, depending on the chosen software:

* Set a matching PHP version — some legacy Apps need older versions
* Enable/disable PHP extensions — for best performance
* Set a [root path](#toc-root-path) — to serve the App from the right location
* Populate [ENV vars](env-vars) — to connect to to the database automatically

So, the Software Preset saves you some work and helps to prevent errors. It does not install anything. It's non-destructive, you can change all settings later on as you wish. This is especially handy with modern software that supports [ENV var](env-vars) configuration and environment detection — like [Laravel](install-laravel) or [Craft CMS](craft-3-deploy-git) do. It makes the application really portable, you can deploy the same code base to any App and it will work out of the box. 

## Settings

Each App comes with a set of basic [features](https://www.fortrabbit.com/specs) and [limits](/limits):

* a dedicated [Git repo](/git-deployment)
* a unique [App URL](#toc-app-name)
* custom [metrics](#toc-metrics)
* included monthly traffic
* [collaboration](collaboration) options
* various other settings

Most of it can be viewed and edited in the [Dashboard](/dashboard). Here is what you'll need to know:

### Domains

There are various settings to control the routing of domains. Please also see the [domains help article](/domains).

### App Name

The App name identifies your App. It is used in many instances — for example to login by SFTP or SSH. You will also find that App name on your invoices.

For quick boarding, your first trial App will have a generic App name, depending on the software you'll use, it will look something like this: `wordpres-4wm7`.

While creating your (second) fortrabbit App you will be asked for an **App Name**. Now, you can choose a name that is easier for your to identify your project like: `bobs-cool-app`. The App Name must be URL friendly (lowercase, no special chars, no spaces) as it will be your test domain as well, the App will be available under: `bobs-cool-app.frb.io`.


#### Changing the App Name

Please note, that it is not possible to change the App Name later on for technical reasons — cool URLs don't change, anyways. 

This is not a bummer. Remember that the App Name is only an identifier to login and for development. The [App URL](#toc-app-url) is front-facing, but you'll only use to develop, you will route your [own domains](/domains) later on anyways.

To identify the App within the Dashboard, you can also use App notes.

If you are really desperate for your own vanity App Name: delete your current App and start a new one, this time using the desired App name. Apps are disposable, so deploying everything again shouldn't take too long.

#### Re-using App names

Sometimes you can't get it to work and you want to start over again. So you can delete your App and create a new one from scratch. Please mind that you can not use the same App name immediately. It takes 7 days after you have deleted an App, that the same App Name will become available to use again.

### App URL

The most prominent use of your App name is your default App URL which looks like this: `https://your-app.frb.io/`. This is where you can always reach your App thru the web browser. Use the App URL for development, testing and to connect to external services. Later on your App can also be reached through your [own top-level-doamin](/domains). The App URL can't be removed or renamed. Check out our [htaccess article](/htaccess) on forwarding and redirecting this URL.


#### Sending transactional mails from your App URL

Some clients asked how to setup TXT records for App URLs. They have the idea of sending mails over a transactional mail service from the App URL, for testing or even for production.

And it is probably not what you want. At the end of the day, you want your mails to be sent from own domain. transactional mail providers need to take extra care that you are not a spammer, as they give very powerful tools. our generic App URLs do NOT look good from their point of view. For the App URL there isn't an MX record. So that sub-domain looks suspicious to any mail-server.

We are using postmark app. It should work the same as mailgun or any other transactional mail provider. We have configured and verified our prime domain to be used with the service. Now, our dev and staging environments are also using the "real thing", but is configured (in our code base) so that they are sending all mails to "info@fortrabbit.com" (instead of what is entered by the testers).

How other users are testing mails: some clients are using mailtrap to test mails. I have no experience. but as far as I know, it's easy to setup and use.


#### Set up a custom domain

You can register your App to accept requests from any external domain you route to fortrabbit — see also [the domain article](/domains). To set up a domain routing, you add a new custom domain within your Apps domain settings in the Dashboard.

<div markdown="1" data-user="known">
* [Set up a new domain for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/new)
</div>

#### Root path

Per default all the [domains](/domains) of the App will route to the same root path (sometimes this is also called: document root, docroot or root folder): `htdocs`. This path is, where the first `index.php` will be called, when people are visiting your App on any domain. This path setting can vary, depending on what the framework or CMS you have selected in the software chooser when creating the App:

| Framework/CMS                                | Root path     |
| -------------------------------------------- | ------------- |
| Laravel, Phalcon, Craft 2, Symfony 4, Slim 3 | htdocs/public |
| Symfony 2, Symfony 3, Craft 3                | htdocs/web    |
| Drupal 8, WordPress, Grav                    | htdocs        |


You can however set the root path afterwards at any given time by visiting the according setting in the Dashboard under your App.

<div markdown="1" data-user="known">
* [Change the root path for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


#### Individual root paths per domain

In some cases, you might want to route individual domains to different folders. Please mind that there are limited use cases for this and don't use one App to host multiple website (see [above](/app#toc-one-website-per-app) as well).

When visiting the root path settings you will find a switch to change from a global root path for all domains to custom root paths for each domain. Mind that you can also use `.htaccess` files with `RewriteRule` directives to handle different domains differently.

### Firewall

Most connections calls on most ports are forbidden for [security](security) reasons. You can request to white-list a port or port range. With the firewall setting of your App you can see custom white-listed ports and request a new firewall rule.

* [List of open ports](https://www.fortrabbit.com/specs#firewall)

### Metrics

Each App comes with "**Usage metrics**": Those show you the current status of the Web storage, MySQL storage and Object Storage and context of your

Some Apps also come with "**Performance metrics**": Those show you how fast your App is and where you might need to improve. Performance metrics are: requests, PHP response time, traffic, errors and other useful stuff. You can set different time intervals to monitor performance over time.

* https://dashboard.fortrabbit.com/apps/{{app-name}}/metrics

### App team

The team settings of the App are showing you which other developers have access to the App. From here you can also invite new team members directly to the App.

See our [collaboration article](/collaboration) to learn more about joint development.

### App Company

This setting links to the Company owning the App. From here you can also:

#### Changing App ownership

1. Go to the App in the Dashboard
2. Click the "Change ownership" button
3. Choose a different Company (and Billing Contact)

The `Change ownership` button, linked from your App overview is a very powerful tool. You can move the App to a different [Company](/company) or [Billing Contact](/billing-contact) you have access to. That means, you can assign the App to a different team, and or to be billed from a different [Billing Contact](/billing-contact).

This action can only be done by [Accounts](/account) who have Owner or Admin privileges on at least two [Companies](/company) or one Company with two [Billing Contacts](/billing-contact). In other words, you need to have something first, to change it to. Also the App must be paid, not in trial mode.

The App will be billed on the old Billing Contact until that day of change and from the next day on to the new Billing Contact. For example: If you move your App on the 15ths of a month to the Billing Contact of another Company, it will be billed until the 15ths to the old Billing Contact and starting from the 16ths to the new Billing Contact.


#### Inviting a client to take over an App

Are you working on behalf of someone? You want your client or the boss of the agency to pay for hosting? Here you can start the invitation. See our [client collaboration](/client-collaboration) article for more details.


## The App trial

You can test fortrabbit for free. Therefore each Account can have one trial App running. The purpose of the trial is test-drive the platform. See if it works as advertised. See if fortrabbit is the right hosting solution for you.

### Starting a new trial App

Find the "Create an App" on the Dashboard home. When asked for the plan, you can choose the free trial option instead. Trial Apps are available for both stacks, Universal & Professional.

### Trial limits

The trial App scaling specs are matching a small preset, always enough to get started. For the Pro Stack not all Components are enabled. The trial is NOT a forever-free tier. An urgent limitation is, that the trial has a limited lifetime. 

### Extending the trial

The default trial time is short. A timer in the Dashboard with your App will show you the time left. You can extend the evaluation period by completing some simple tasks like:

* Confirming your Accounts e-mail address
* Setting your real name under your Account
* Deploying some code to the trial App
* Routing a domain to the trial App

A task that is completed has a check-mark `✔`. Tasks that are still todo will show how much time you will gain more time.

Additionally, after some trial time has passed, we show a link to a contact form where you can ask us to extend the trial time. Here you'll write us something about your goals with the trial and we are happy to extend the trial for much longer period. You can see those tasks in the Dashboard with the overview of your App.

### Upgrading the trial

You can, of course, upgrade from your trial to a paid plan at any time. You can do so under your Apps overview.

### The end of a trial

When the trial is finally over, the trial App will be deleted. That might sounds cruel, but please consider that: We will not keep your data hostage until you pay us. 

Good news is: we will inform you by e-mail before it happens and you can start a new trial right away, after your current trial ended. In many cases you can easily re-deploy your code base to the new App: When deploying with [Git](git-deployment), you just change the remote and push again. When using our CMS/framework presets with predefined [ENV vars](env-vars) database access will work without code modification.



## Deleting an App

You can always delete Apps you own or admin. To delete an App, visit the App in the Dashboard, find and hit the "delete" button. You might need to enter your Account password and also confirm what you are about to do. The App will then get deleted within a couple of minutes. This includes all files and the database.

### Recovering deleted Apps

Deleting Apps & Accounts is "mostly" (see [below](#toc-recovering-backups)) FINAL here and can't be reversed. We delete as much as possible when a client requests so or we had to when payments bounced to often. We do so for privacy and security reasons. We believe that it should be your right to be forgotten. Imagine that we take your data as a hostage until you pay us. So in most cases that's actually good. Also read [how we handle bounced payments](/billing#toc-bounced-payments).

### Recovering Backups

While the App get's deleted irrevocable, Backups might still be around. When your App plan included [Backups](/backups-uni) and those have not yet faded out within their retention period, we might can restore the backups for you. We then can provide you tar ball containing your data (web + mysql). Please contact us for that via chat.





## Troubleshooting

Here are some common gotachs that mostly happen with new Apps:

### 404 Not Found

When your App is just sending a "404 file not found" page, please check this before contacting support:

#### No code deployed

A common case for the 404 page is, that no code has been deployed yet. 

This can be misunderstanding, you might thought with choosing a [Software Preset](#toc-software-prest) software will be installed. Sorry, no one-click-installs here, you need to install the software yourself. So please go ahead deploy some code first.

Maybe also, the deployment is not yet finished (SFTP is still uploading?) or your initial Git push returned an error? Please check if all code is deployed completely. With a Universal App you can use SSH/SFTP to login and see if the files are there.

#### Wrong root path

Maybe your software is using a different root path than the one that is currently set? Check the [root path](#toc-root-path) settings.

`htdocs` is the default [root path](#toc-root-path), when no specific software has been chosen in the [Software Preset](#toc-software-preset). Now, when you decide to install [Laravel](install-laravel) or any software later on, you might to set the root path accordingly.

We have also seen cases where people have uploaded a whole `Craft` folder into the Apps `htdocs` folder. You should probably better upload all files into `htdocs` directly not into an extra folder that contains the files.


#### .htaccess is missing

Another common cause for 404 errors is a missing `.htaccess` file. This file is hidden from your Operating System by default (as it starts with a point) but contains important rules for your application to function properly.
So when you are uploading with SFTP and you have maybe dragged the files from your Desktop (Finder) into your SFTP application (Cyberduck, Transmit, FileZilla), this file is likely missing. 

You might can use the file explorer from your SFTP application or temporary show hidden files in your OS to make the `.htaccess` visible to you. Just make sure that when a htaccess is present (most likely it is), that it get's uploaded as well.

#### App is not yet ready

Creating an App can sometimes take a few minutes. When you visited the App URL during that time, you'll get also get an 404 error. It's possible that this DNS response get's cached locally. 


### 500 Internal Server Error

Your application is just throwing a 500 or 502 internal server error? Congrats, at least something is happening. In most cases, this is NOT a server failure, it's a software problem caused by your configuration. And it's something you can likely solve yourself.

There are many different reasons for 5xx errors. Be a detective and start the investigation by examining the logs, see [here for Uni Apps](logging-uni) and [here for Pro Apps](logging-pro). Within the logs you will find where application exited with which error, this is the hot leas you are after.


### Version mismatch without any changes made

Sometimes service reboots are issued to Apps. This usually doesn't happen often, but sometimes needs to be done in case of an incident, a scheduled maintenance or a re-distribution. Now, with these service restarts, the orchestration logic will look for an existing Git repo and re-deploy that.

Depending on your workflow, that might cause some strange results on Universal Apps. 

As you hopefully know, we originally assume that you have a local development and that this is your "master" and all changes are done there first. Also, the core code is always deployed with Git and thus Git is up-to-date and that re-deploy should therefore do no harm. So when you have been following our guides, everything should work for you.

The strangeness occurs when you are mixing workflows and are going offroad: For example, you have initially deployed by Git and then later updated your CMS directly on the App itself from the Control Panel in the browser. Using the [overwrite but not delete strategy](deployment-methods-uni#toc-git-push-overwrite-but-not-deletes) the old files from Git get re-deployed. This can get messy, when the files are outdated but the database is already migrated to the newer version. We also saw this overwriting session and cache files (which might be excluded from Git anyways).

The fix for that depends on your specific situation. In many cases we suggest to update your installation and bring that in order and then re-deploy the changes via Git one more time.


### phpinfo to the rescue

Sometimes, you might be unsure on configuration settings such as which extension in which version are exactly enabled or which ENV vars are actually available with your App. You can use `phpinfo` (see [offcial doc](https://www.php.net/manual/en/function.phpinfo.php)) for that. Place a "info.php" file with the content `<?php phpinfo(); ?>` anywhere in your public accessible path and call that URL with the browser to see the config dump.

We are maintaining standard PHPinfo Apps as well here: [fortrabbit.com/specs#php-extensions](https://www.fortrabbit.com/specs#php-extensions). Your CMS / framework might have it's own dump of that. With Craft CMS for example you can visit a nicely formated phpinfo in the control panel. 

**A word of warning**: Don't do that in production. Make sure to delete that file right away after you have finished your investigation. The output might contain sensible information you don't want to share with the world. Key and values of your ENV vars are visible, that can contain database access and API keys to external services.


## Further readings

Now you know the basics about Apps on fortrabbit, keep going:

* [Quirks and constrains](/quirks)
* [App Specs](https://www.fortrabbit.com/specs)
* [About the Stacks](/stacks)
