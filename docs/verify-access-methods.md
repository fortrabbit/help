---

template:      article
reviewed:      2021-03-25
naviTitle:     Access testing
title:         Testing access to fortrabbit services
lead:          This article provides instructions on how to check code access for username + password and SSH key authentication from the terminal using sftp and ssh.
group:         troubleshooting
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

Unsure which identification method you are using? See [here to identify your access method](access-methods#toc-identify-your-current-access-method).


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
$ ssh tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
app
```

You are good to go when the output is "app" for the following command is "app".



## Test access for SSH key authentication

```nohighlight
App Name  tripple-w-app (example)
Region    eu2
Username  tripple-w-app
Host      deploy.eu2.frbit.com
Password  None or whatever you used before (not fortrabbit Account password)
```

Open a terminal and enter:

```bash
$ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
app
```

You are good to go when the output is "app" for the following command is "app".

If you see a password prompt, then you either created the key with a passphrase or the key was very recently imported and the system thinks you are still using the username + password mode.

Either type in the passphrase that was used when the key was created or allow the system up to 7 minutes to activate the new public SSH key.