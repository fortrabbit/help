---

template:      article
reviewed:      2021-03-19
naviTitle:     SFTP troubleshooting
title:         SFTP troubleshooting
excerpt:       What you may want to know to debug SFTP issues
lead:          'This article helps solving common issues when trying to connect by SFTP to on an App hosted on fortrabbit.'
group:         troubleshooting
stack:         all

keywords:
     - ftp
     - username
     - password
     - access
     - authentication

---


## Common reasons and solutions

Are you unable to login to your Universal App using SFTP? Please scan the table of contents here to see if your problem is already covered.


## Password not accepted

Are you seeing a "**Permission denied, please try again**" error?

### Check your access method first

Make sure that you know which access method you are currently using. We have "username + password" or "SSH public key authentication". If you are unsure, please please see the section about [figuring out which access method is currently active](access-methods#toc-available-authentication-methods) for your fortrabbit Account.


### Tips to proceed when username + password is enabled

If you are using the username + password access mode, then you need to type in your fortrabbit Account password when authenticating with SFTP. The username will look like `{{appname}}.{{randomletters}}`. 

Make sure to copy-paste the correct credentials for hostname and user from the Dashboard. Find the Access to your App.


### Tips to proceed when SSH public key is enabled

If you are using the SSH public key authentication and still face a password prompt it can mean a few things. 


**Essentially, only one access mode can be active at any particular time.** This is explained on the [access methods page](access-methods).
There is a [test on the sftp-access page for the ssh-key access mode](sftp-uni#toc-verify-ssh-key-mode) that you can perform from a terminal.
Do it, and you will know more about your situation.


#### Recently imported key not working

After importing a key in the Dashboard, please allow up to 7 minutes for it to be activated. If you try to access the App before the SSH key has been activated, you will be asked for a password, because the old "username + password" is still in effect.

#### Internal error I01 after an App is created

If the App was recently created, please wait 60-180 seconds for it to become ready.
Trying to git-push or to shell in before the App is ready produces an error like shown below.
If more than five minutes have passed and you still get a similar error (despite
successful authentication), then please contact us via the support-chat.

    A B O R T E D

      !! Internal error I01 - please try again later.

    Connection to deploy.eu2.frbit.com closed.

### Connection hangs for a long time

If nothing happens for a long time when you shell in via SSH or try to login via SFTP,
then your IP address is probably quarantined.
This will happens when a client fails to to authenticate successfully several times within
a short period of frame.  If you think your IP is blocked, [check your options](denylist).

### FTP is not working

We do not support FTP. Please pick SFTP instead.

### I can not login to my Pro App by SFTP

There is no SFTP for Pro Apps. There is only [remote SSH execution](/remote-ssh-execution-pro).


## Related topics

- [Code access methods](/access-methods) on fortrabbit
- [Deployment methods](/deployment-methods) on fortrabbit
- [SFTP access](/sftp-uni)
- [SSH access troubleshooting](/ssh-troubleshooting)
- [SSH key setup](ssh-keys)
- [SSH key troubleshooting](/ssh-key-troubleshooting)
