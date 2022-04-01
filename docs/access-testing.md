---

template:      article
reviewed:      2022-04-01 14:51:31
naviTitle:     Access testing
title:         Testing access to fortrabbit services
lead:          This article provides instructions on how to check code access for username + password and SSH key authentication from the terminal using ssh.
group:         access
order:         2
stack:         all

dontList:      false

keywords:
     - ssh
     - ssh key
     - username
     - password
     - access
     - authentication

---


People often report authentication issues, but code access via SSH, SFTP and Git is rarely broken on our end. Follow these steps to test and verify that the connection works. Should it turn out otherwise, at least you will be able to point our support to what is failing.


## Prepare

For the following examples we use a theoretical App called `tripple-w-app` that is hosted in the `eu2` region. Replace those values with the ones from your own App. Best look up the credentials from the Dashboard > Your App > Access.

Unsure which access method you are using? See [here to identify your access method](access-methods#toc-identify-your-current-access-method). Test the one that is active for you.



## Test access for username + password authentication

```nohighlight
App:        tripple-w-app
Region:     eu2
Host:       deploy.eu2.frbit.com
Login:      tripple-w-app.f4n4gkrx90ot4yxm
Password:   Your fortrabbit Account password
```

Open a terminal and enter:

```bash
$ ssh tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com -F /dev/null whoami
app
```

You are good to go when the output is "app".

When this is not working for you can try the following:

* Do you have other Apps hosted here? Try to access these.
  * If you can access other Apps and it is just not working for a specific App and that App is older than 5 minutes, <a href="#asd" onclick="Intercom('showNewMessage', 'I can not access my App ______.')">contact support</a>.
  * If you can not access any App at all, it's likely something on your side continue below
* If you don't have other Apps hosted here and this is the first time you are doing this, it likely is related to the credentials you are entering. Make sure to use the correct values.
* If you are using the classical cmd.exe terminal in Windows, use the mouse to paste or type your password. Read more [here](https://superuser.com/questions/62598/how-do-i-paste-into-the-windows-cmd-prompt-by-using-only-the-keyboard) and [here](https://www.maketecheasier.com/enable-copy-paste-command-prompt-windows10/)
* Reset your fortrabbit Account password with the fortrabbit Dashboard under your Account and wait a short while until the changes are applied and try again.
* Check for the unlikely case that there is a general service issue under [status.fortrabbit.com](https://status.fortrabbit.com)
* If nothing helps please <a href="#asd" onclick="Intercom('showNewMessage', 'I can not access my App ______. I have already tried ________')">contact support</a> and provide details of steps you have been taking so far.


## Test access for SSH key authentication

```nohighlight
App Name:   tripple-w-app (example)
Region:     eu2
Username:   tripple-w-app
Host:       deploy.eu2.frbit.com
Password:   None or whatever you used before (not the fortrabbit Account password)
```

Open a terminal and enter:

```bash
$ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa whoami
app
```

You are good to go when the output is "app".

If you see a password prompt like this:

```bash
$ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa whoami
tripple-w-app@deploy.eu2.frbit.com's password:
```

Then these two cases are likely:

+ You created your SSH key with a passphrase. Type in the passphrase.
+ The SSH key was very recently imported. Allow the system up to 7 minutes to activate the new public SSH key.

If you still have trouble please proceed to the [SSH key troubleshooting page](/ssh-key-troubleshooting).
