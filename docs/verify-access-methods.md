---

template:      article
reviewed:      2021-03-23
naviTitle:     Verify access methods
title:         Testing access to fortrabbit services
lead:          This article provides instructions on how to check code access for username + password and SSH key authentication from the terminal using sftp and ssh.
group:         troubleshooting
stack:         all
dontList:      true

keywords:
     - ssh
     - username
     - password
     - access
     - authentication

---

## Verify access methods

People report authentication issues quite often, but SSH access and SFTP access is rarely broken on our end. Follow these steps to verify that the connection works. Should it turn out otherwise, at least you will be able to point to what is failing.

For the following examples we use a theoretical App called `tripple-w-app` that is hosted in our `eu2` region. Replace those values with the ones fro your App. Best look up the credentials from the Dashboard > Your App > Access.


### Verify access using username + password authentication

Username + password is the default access mode on fortrabbit and used when you have not set up SSH keys. See [here to identify your access method](access-methods#toc-identify-your-current-access-method).

```nohighlight
App:        tripple-w-app
Region:     eu2
Host:       deploy.eu2.frbit.com
Login:      tripple-w-app.f4n4gkrx90ot4yxm
Password:   Your fortrabbit Account password
```

To verify that SFTP access works, open a terminal and type the following, but use your own App name and the correct region. If you are using a specific framework such as Craft CMS or Laravel, then before running the commands, go to the root folder of the App first. That folder is usually called `public` or `web` (depending on system).

```bash
$ unset SSH_AUTH_SOCK
$ echo hello world > f
$ sftp -F /dev/null tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com
tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com's password:
```

Paste or type your password, output continues...  
When you get to `sftp>`, type put f, enter, exit, enter.

<!--
The part above is not clear to me.
-->


```bash
Connected to deploy.eu2.frbit.com.
sftp> put f
Uploading f to /srv/app/tripple-w-app/htdocs/f
f                                         100%   11     0.3KB/s   00:00
sftp> exit
```

Now access the file with an HTTP client like so:

```bash
$ curl -D- https://tripple-w-app.frb.io/f
HTTP/1.1 200 OK
Date: Thu, 18 Mar 2022 09:50:21 GMT
Server: Apache 2.x
Last-Modified: Thu, 18 Mar 2022 09:43:32 GMT
ETag: "b-5bdcc6ebed7b6"
Accept-Ranges: bytes
Content-Length: 12

hello world
```

You can also use your web browser to visit `https://tripple-w-app.frb.io/f`.


### Verify ssh-key access mode

Given that you have an App named  _tripple-w-app_ in EU, then the following applies. If you are using a specific framework such as Craft-CMS or Laravel, then before running the commans, go to that folder first. That folder is usually called public or web. Change the App name and the region according to your own parameters.

<!-- fixme: this is detected as PHP -->

```nohighlight
App Name  tripple-w-app (example)
Region    eu2
Username  tripple-w-app
Host      deploy.eu2.frbit.com
Password  None or whatever you used before (not fortrabbit Account password)
```

Try this, if the output is "app", then you are good to go.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
    app

If you see a password prompt, then you either created the key with a passphrase or the key was very recently imported and the system thinks you are still using the username+password mode.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
    tripple-w-app@deploy.eu2.frbit.com's password:

Either type in the passphrase that was used when the key was created or allow the system upto 7 minutes seconds to activate the new key.
