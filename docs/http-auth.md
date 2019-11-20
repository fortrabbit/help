---

template:    article
reviewed:    2019-09-30
title:       Using HTTP Auth on fortrabbit
lead:        You probably don't want the whole world to see your development in progress. Better restrict access to a fortunate few. This is how to use HTTP (basic) authentication to trigger a username/password prompt in the browser.
naviTitle:   HTTP auth
group:       platform
stack:       all

keywords:
    - HTTP Auth
    - password for website
    - php

---



## Set a root path below htdocs

When no framework or CMS was chosen when creating the App, `htdocs` is the default root folder of the App. Create a new folder, called something like `web` or `root`. Move all your web contents into that folder. Set the newly created folder containing everything now, as the new root path in the Dashboard, see also [here on how to do the last part](/app#toc-root-path). This step not required, as with fortrabbit all files starting with: `.ht*` will be blocked from delivery.


## Create the .htaccess file

Create a `.htaccess` file in the directory you want to secure, also see our [.htaccess article](/htaccess). Likely that this is your new root directory, can also be below that. Be careful, the dot at the beginning of the file names indicates that it is a hidden file. You can upload the file by any deployment method: with Git, by SFTP or create it on remote via SSH. Please note, that all frameworks and CMS systems will already have an `.htaccess` file in their own root path. You can modify that as well. This is, what your `.htaccess` should contain for HTTP auth:

 
```
Authtype Basic
AuthName "Welcome to my awesome project. Please Login."
AuthUserFile /srv/app/{{appname}}/htdocs/.htpasswd
Require valid-user
```

The first line starts the directive. The second line set a hello message which is printed to greet the user. The third line points the server to the file that stores the username and password to login, this is an absolute path. This `.htpasswd` file should NOT be reachable from the outside, via http. That's why you have moved all the files one level deeper. The last line finally sets the rule, that only valid users are allowed to browse.

## Create the .htpasswd file

As you can already guess by now, the `.htpasswd` file hosts the required username and password to enter your project. And this is how that file can look like:

```plain
john:$aSr1$jw6nGOHx$f/HpoNv9thNMd6w35Ttl80
```

The username is before the colon. The long string after the colon is the password. It needs to be Base64 encrypted. "htpasswd" is a command line tool likely installed on your local machine (sorry not on fortrabbit yet, so you have to run that locally):

```bash
htpasswd -c ~/directory/.htpasswd john
```

The -c flag create a new file. The first param defines the file name and (absolute) pat to store it. The second param the username. The tool will prompt for the password. You can also use web service like this one:

* [htaccesstools.com/htpasswd-generator](http://www.htaccesstools.com/htpasswd-generator/)




## Deploy HTTP Auth

So now, as you know how to create a password protected area for your website, you need to figure out how to implement it. Most likely you want this for a "staging" area — a version of your website that is publicly served, but not intended to be open for everyone. That can be your website before it get's live or even a dedicated permanent staging area. Please see our [multi staging article](/multi-staging) for more on this. 

Likely, your goal is, to ask users for a password only on the staging area, but not on your local environment or even a live version. Unfortunately HTTP Auth does not play very well with standard practices of [environment detection](/local-development#toc-environment-detection), as this is happening on a different layer. Here is what you can do:

### Upload via SSH / SFTP

You can upload the two files `.htaccess` and `.htpasswd` directly via SSH or SFTP into your App on fortrabbit. This is not very programmatic, but pragmatic. And it works. This applies only to the Universal Stack with persistent storage. 

### Create while deploying

When deploying with Git, you can use a post-deploy hook, to trigger automatic generation (or placement) of the required files. This applies to the Universal and Professional Stack Apps.


## Alternative solutions

HTTP Auth is a basic, standard, minimally invasive solution. There is no UI and no dependency you need to take care of. Even database access is not required. That's the beauty of it. So it's most likely compatible with the rest of your application. But HTTP Auth is not the only way to protect your website with username and password. Your CMS or framework might bring their on solutions for this. There are plenty plugins for Craft CMS and WordPress. 


## See also

Check out our [htaccess](/htaccess) article for more hands-on examples.
