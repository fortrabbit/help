---

template:      article
reviewed:      2021-03-23
naviTitle:     Verify access methods
title:         Testing access to fortrabbit services
lead:          Instructions on how to check access from a terminal
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

### Verify access using username + password authentication mode

This is te default access mode. If you have an App named tripple-w-app in the eu2 region, then the following applies:

```nohighlight
App      tripple-w-app
Region   eu2
Host     deploy.eu2.frbit.com
Login    tripple-w-app.f4n4gkrx90ot4yxm
Password Your fortrabbit Account password
```

To verify that SFTP access works, open a terminal and type the following, but use your own App name and the correct region. If you are using a specific framework such as Craft-CMS or Laravel, then before running the commands, go to that folder first. That folder is usually called public or web.

    $ unset SSH_AUTH_SOCK
    $ echo hello world > f
    $ sftp -F /dev/null tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com
    tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com's password:

Paste or type your password, output continues...  
When you get to `sftp>`, type put f, enter, exit, enter.

    Connected to deploy.eu2.frbit.com.
    sftp> put f
    Uploading f to /srv/app/tripple-w-app/htdocs/f
    f                                         100%   11     0.3KB/s   00:00
    sftp> exit

Now access the file with an HTTP client (web browser)

    $ curl -D- http://tripple-w-app.frb.io/f
    HTTP/1.1 200 OK
    Date: Thu, 18 Mar 2021 09:50:21 GMT
    Server: Apache 2.x
    Last-Modified: Thu, 18 Mar 2021 09:43:32 GMT
    ETag: "b-5bdcc6ebed7b6"
    Accept-Ranges: bytes
    Content-Length: 12

    hello world

### Verify ssh-key access mode

Given that you have an App named  _tripple-w-app_ in EU, then the following applies. If you are using a specific framework such as Craft-CMS or Laravel, then before running the commans, go to that folder first. That folder is usually called public or web. Change the App name and the region according to your own parameters.

<!-- fixme: this is detected as PHP -->

    App Name  tripple-w-app (example)
    Region    eu2
    Username  tripple-w-app
    Host      deploy.eu2.frbit.com
    Password  None or whatever you used before (not fortrabbit Account password)


Try this, if the output is "app", then you are good to go.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
    app

If you see a password prompt, then you either created the key with a passphrase or the key was very recently imported and the system thinks you are still using the username+password mode.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
    tripple-w-app@deploy.eu2.frbit.com's password:

Either type in the passphrase that was used when the key was created or allow the system upto 7 minutes seconds to activate the new key.
