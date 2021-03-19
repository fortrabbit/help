---

template:      article
reviewed:      2020-11-28
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

**Related topcs**

- [Code access methods](/access-methods) on fortrabbit
- [Deployment methods](/deployment-methods) on fortrabbit.
- [SFTP access](/sftp-uni).
- [SSH access troubleshooting](/ssh-troubleshooting)
- [SSH key setup](ssh-keys)
- [SSH key troubleshooting](/ssh-key-troubleshooting)

## Common reasons and solutions

If you are unable to log in to your Universal-Stack App using SFTP,
then scan the table of contents here to see if your problem is already covered.

## Password problems (aka Permission denied, please try again)

First of all, [figure out which access method is currently active](access-methods#toc-available-authentication-methods) for your fortrabbit Account.

### Option 1: **password+username mode**

If you are using the username+password mode, then you need to type in your fortrabbit account password when authenticating with SFTP.
The username will look like «app-name».«random-letters». Find your App in the Dashboard to figure out this login / sftp-username.
The sftp-acces page provides a [test for the password mode](sftp-uni#toc-verify-username-password-mode) you can perform from a terminal.
Do give that a try.

### Option 2: **ssh-key mode**

If you are using the ssh-key mode and still face a password prompt it can mean a few things.

- your ssh key is protected by a passphrase
- the rsa key in standard location (~/.ssh/id_rsa) has been rejected
- all keys in your ssh-agent have been rejected

If you key is protected by a passphrase, then type that in.
We do not know and can not know what you picked as the passphrase for your key.
It may or may not be the same as your fortrabbit Account password.

If your keys have no passphrases or if they are already unlocked in the agent but have all been rejected, then the ssh-server will still ask you for a password.
This is because it must support both access modes. If you are using the ssh-key access mode, typing in any password will not grant you access, even if you type in your fortrabbit Account password.
**Essentially, only one access mode can be active at any particular time.** This is explained on the [access methods page](access-methods).
There is a [test on the sftp-access page for the ssh-key access mode](sftp-uni#toc-verify-ssh-key-mode) that you can perform from a terminal.
Do it, and you will know more about your situation.


#### Recently imported key not working

After importing a key in the dashboard, please allow upto 3 minutes for it to be activated. If you try to access the App before the ssh-key has been activated, you will be asked for a password, because the old password+username is still in effect.

    $ ssh tripple-w-app@deploy.eu2.frbit.com ls -lha
    tripple-w-app@deploy.eu2.frbit.com's password:

The passphrase may be chosen when the key is created, but it can aslo be blank. If your key was generated without a passphrase, typing the dashboard fortrabbit account password here will not do what you want.  For the username+password mode to work, you need to remove all ssh-keys from your profile.



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


