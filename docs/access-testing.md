---

template:      article
reviewed:      2021-03-26
naviTitle:     Access testing
title:         Testing access to fortrabbit services
lead:          This article provides instructions on how to check code access for username + password and SSH key authentication from the terminal using ssh.
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
$ ssh tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
app
```

<!--

Sorry to bother now with this. The -i flag with the ssh command says to use the id_rsa_fortrabbit key, right? But we can not know if that is installed for sure. 

It is only part of our help to set it up like that. I guess most people will use keys that they already have and that are not called id_rsa_fortrabbit. We do have a GitHub auto-import. I think many people are using that.

So I suggest to remove that part (from both examples).

-->

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

You are good to go when the output is "app".

If you see a password prompt, then these two cases are likely:

+ You created your SSH key with a passphrase. Type in the passphrase.
+ The SSH key was very recently imported. Allow the system up to 7 minutes to activate the new public SSH key.


<!--

I am missing what people should do next? After they have done the test and it is not working? I guess they should use verbose mode next? The SSH troubleshooting guide good be mentioned next. But I am not how the connection will be.

-->



<!--

Sorry for further tearing apart your document here. I realize that there is not much left from it's original content.  Last comment. I swear:

Let's say we get the dynamic variables fixed, so that they show up correctly when the page loads. That was the intention with those vars, we could skip the step where the client needs to figure out what their access method is. We can show the {{ssh-user}} and that's it.

-->
