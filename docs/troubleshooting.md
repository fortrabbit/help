---

template:      article
reviewed:      2025-07-09 14:40:31
title:         Troubleshooting index
naviTitle:     troubleshooting
excerpt:       Hanging somewhere? Something is not working? Check out the most common gotchas here.
lead:          New to fortrabbit? Here is a collection of common problems when getting started. 
group:         platform
stack:         all
dontList:      true

---


## It's not working!

That's a very vague error description. Please exactly define what "it" is! In general, please make sure to have read and followed our [help articles here](/) and especially the install guides section. Step by step instructions to successfully deploy your application are included. You can also search for your topic of interest.


## I can not connect via SFTP or SSH!

This can have multiple causes. Please make sure to grab the correct access credentials from the Dashboard for your App. See our [SFTP article](sftp-uni) for more on that. We offer [two access methods](/access-methods): SSH key and password. See our [SSH key troubleshooting guide](/ssh-keys) when using the later. Please also mind that SFTP and SSH login is only by [Universal Apps](/app-uni). [Professional Apps](/app-pro) have remote SSH execution, see [here](remote-ssh-execution-pro).


## I can not push code by Git!

Please also the question above on authentication. Check if SSH/SFTP is working for you and if Git is setup correctly. Also see our [getting started with Git article](/git), when you are new to this. Check out the [Git troubleshooting](/git-troubleshooting) section.


## Composer update is failing!

That's likely because you have tried to run the Composer update on the App itself? That's not the way it is supposed to work for most workflows. Please also see the [Composer article](/composer) and especially the section on [using Composer via SSH](/composer#toc-composer-from-ssh).


## Old files are not deleted!

When deploying with Git, you might wonder why some old files have not been deleted from the Apps web storage, although you have deleted them from Git. That's not a bug, it's a feature. The [Universal Stack](/app-uni) has a persistent web storage, therefore the App can change the file system, like when uploading images. The Git repo and the web storage are not the same thing, so those changes will not be reflected in the Git repo. That's, why with the Universal Apps, the Git deployment uses an overwrite but not delete strategy. Changes will be applied, nothing will get deleted. You can login to that App by [SSH](/ssh-uni) or [SFTP](/sftp-uni) and delete the files manually. See more details in our [deployment article](deployment-methods-uni#toc-git-works-only-one-way).


## I see a 403 error

See [here](/403-errors).

## I see a 404 error

See [here](/404-errors).


## My domain is not working!

That can have multiple causes. See the [DNS and domain troubleshooting help](domains#toc-troubleshooting-dns), also make sure to read the info above.


## I see an SSL cert error

See [here](/tls-errors).


## I see a 500 or a 503 error

See [here](/503-errors)

You can likely find out about that yourself. Please see [the App troubleshooting section](/app#toc-500-internal-server-error) for more.


## I see a 504 error

See [here](/504-errors)


## I uploaded Craft by SFTP but it's not working!

Most likely you have not followed our install guides. We expect you to have a [local development environment](/local-development) and fortrabbit to be production. Please see the [Craft SFTP guide](/craft-upload-sftp#toc-service-unavailable-error).


## I clicked on my desired software but it got not installed!

That's the expected behavior. The software preset will only set some defaults. You will install the software yourself. More on that [here](/app#toc-software-preset). Please follow your specific [install guides](/#install-guides).


## The password reset is not working

Maybe you have used a different e-mail? Check if that is the correct e-mail. Or maybe you have setup 2FA earlier? Then you need to 1st just enter something with e-mail / password and then click on the "looking for 2FA" link. That's a design flaw on our side. Sorry.


## It's still not working!

See what the error says and try to find out the solution yourself. This is a hosting platform for developers. We expect you to be able to help yourself. We just like to help you finding a solution yourself.


## But I still have problems

Do not hesitate to contact us. Make sure to provide all details upfront so that we can back on you easily. See [here](https://www.fortrabbit.com/support-policy#successful-support) on how to get good support quickly.


## Blocklisting

See [here](/denylist)
