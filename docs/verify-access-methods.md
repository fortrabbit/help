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
     - ssh key
     - username
     - password
     - access
     - authentication

---


<!--  
Proposing to change the title from "verify access methods" to "Code access verification" or "Code access testing" or "Access troubleshooting". 

"Verify access methods" means to me to test if both access methods work. But the goal of the user is to get access working, whatever their method is.

I also removed it from being linked from the development section and put it in the troubleshooting section. But also in there I am not sure if it is aligned with the other articles and more like a special article to be used in support? 

-->

People often report authentication issues, but code access via SSH, SFTP and Git is rarely broken on our end. Follow these steps to test and verify that the connection works. Should it turn out otherwise, at least you will be able to point our support to what is failing.


## Prepare

For the following examples we use a theoretical App called `tripple-w-app` that is hosted in the `eu2` region. Replace those values with the ones from your own App. Best look up the credentials from the Dashboard > Your App > Access.

<!--
I still would like to use the {{app-name}} dynamic vars here, because they are easy to identify when not filled correctly. Leaving it up to you. Maybe explain to me why you are using your own example syntax here. 
-->

Unsure which identification method you are using? See [here to identify your access method](access-methods#toc-identify-your-current-access-method).

<!--
I used the prepare block above that applies to both examples to keep it DRY. Change back if ypu want.
-->


## Test access for username + password authentication

```nohighlight
App:        tripple-w-app
Region:     eu2
Host:       deploy.eu2.frbit.com
Login:      tripple-w-app.f4n4gkrx90ot4yxm
Password:   Your fortrabbit Account password
```

To verify that SFTP access works, open a terminal and type the following, but use your own App name and the correct region. If you are using a specific framework such as Craft CMS or Laravel, then before running the commands, go to the root path of the App first. That folder is usually called `public` or `web` (depending on CMS or framework).

```bash
$ unset SSH_AUTH_SOCK
$ echo hello world > f
$ sftp -F /dev/null tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com
tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com's password:
```

Paste or type your password, output continues...  
When you get to `sftp>`, type put f, enter, exit, enter.

<!--
The above part: 
"When you get to `sftp>`, type put f, enter, exit, enter."
is not clear to me. there is an sftp prompt?
"Enter, exit, enter" < what does that mean? The Enter key? The Escape key?
Use <kbd> for that?
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

<!--
I see you don't like to put the output in a comment block, but maybe have a look how it is rendered in the browser. The syntax highlighter put's emphasis on numbers, this is confusing to me. Compare it visually with the output just in grey (via comment).
-->

You can also use your web browser to visit `https://tripple-w-app.frb.io/f`.

<!--
Why not skip the part about uploading a file and calling it via curl (since limits with root path)? Isn't the success message enough?

Also, I don't fully understand why the SSH key access method and the username + password method are not doing the same thing? Why not let both do SSH? Why does username + password need to be tested with SFTP and why must SSH key access need to be tested with ssh commands?

I prefer the second test, since non-destructive and does not leave files on the server.
-->


## Test ssh-key access mode

```nohighlight
App Name  tripple-w-app (example)
Region    eu2
Username  tripple-w-app
Host      deploy.eu2.frbit.com
Password  None or whatever you used before (not fortrabbit Account password)
```

You are good to go when the output is "app" for the following command is "app".

```bash
$ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
app
```

If you see a password prompt, then you either created the key with a passphrase or the key was very recently imported and the system thinks you are still using the username + password mode.

```bash
$ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
tripple-w-app@deploy.eu2.frbit.com's password:
```

Either type in the passphrase that was used when the key was created or allow the system up to 7 minutes to activate the new public SSH key.


