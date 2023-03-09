---

template:         article
reviewed:         2023-03-07
order:            4
title:            Deploy Statamic with Git on fortrabbit (experimental)
naviTitle:        Statamic Git experimental
lead:             Here is an adventurous Statamic Git deployment workflow.
group:            statamic
stack:            uni
dontList:         false

websiteLink:      https://statamic.com/
websiteLinkText:  statamic.com
category:         CMS
image:            statamic-mark-new.svg
version:          3
supportLevel:     b

keywords:
    - statamic
    - starterkit
    - markdown
    - laravel
    - flatfile
    - headless

---

## Get ready

You should have: Statamic running on your local machine, a good understanding of Git and rsync, an App on fortrabbit. Read our [Statamic intro](/statamic) first.

## Experimental Git workflow intro

This is an advanced, opinionated and **experimental** workflow for experienced developers to deploy Statamic, where all content is stored as files and managed with Git using the Statamic Git Automation. See the [Git Automation Statamic docs article](https://statamic.dev/git-automation) to get the idea and as reference.

The idea is to create another repo on the App, which has the same upstream as your local repo: both the local and the App repos are sharing the same upstream. So the first step is to create this repo on the App, then create a new branch there, which is where we put any new content created on the App. (We create this new branch because we do not want to push changes to the master branch on the deploy service, because this would trigger a deployment.) Then, any changes we make to content on the App can then be pulled into your local repo, where it can be merged.

## 1 - Local preparation

1. Install Statamic on your local computer as described above.
2. Then follow the Git guide above, but leave out the `.gitignore` part under 2.a.

After you have finished with the first `git push`, your App should be online and you can login as admin with your user credentials on the App in the same way as you can do on your local computer.

## 2 - Get Statamic Pro

The Git Automation is part of the commercial Statamic version. You need to enable the Pro version and obtain a Statamic license to use that feature on fortrabbit. See the [official guide](https://statamic.dev/licensing) on how to do that.

As usual, do this with your local installation first and then push the latest state to fortrabbit. After this, you should be able to find Git with the utilities under the Statamic control panel.

## 3 - Set ENV vars in the fortrabbit Dashboard

Add the environment variables listed below to the App with the fortrabbit Dashboard.

```.env
STATAMIC_LICENSE_KEY=your-site-license-key
STATAMIC_GIT_ENABLED=true
STATAMIC_GIT_AUTOMATIC=true
```

## 4 - Prepare Git on the fortrabbit App

Login to your fortrabbit App by SSH and:

1. Set up an SSH key with SSH keygen (no passphrase), see our [SSH keys setup guide](/ssh-keys#toc-generate-a-ssh-key-pair-aka-ssh-key-)
2. Copy the content of the public key (likely `/srv/app/{{app-name}}/htdocs/.ssh/id_ed25519_fortrabbit.pub`) to a buffer
3. In the fortrabbit Dashboard create a new 'app-only SSH key' with the App, paste the contents of the public key file (open the file with Vim)
4. While logged in by SSH create a config file in the already existing folder `.ssh` by running `touch config`
5. Add the content below to the newly created config file (edit the file with Vim)

```config
# Add this to .ssh/config
# Use the App specific region, app name and SSH key name
Host deploy.{{region}}.frbit.com
    IdentityFile /srv/app/{{app-name}}/htdocs/.ssh/id_ed25519_fortrabbit
```

## 5 - Init Git on the fortrabbit App

We need to create a separate branch to avoid triggering a deployment when pushing to the deploy service. So, make sure you are still logged in by SSH on the fortrabbit App.

First add a `.gitignore` file. Its contents should be the same as the `.gitignore` file on your local project. Then:

1. Initialise a new git repo: `git init`.
2. Create a Git user (see example below)
3. `git add .` & `git commit -m 'initial commit'`
4. Create an `editorial` branch and check it out (`git checkout -b 'editorial'`)
5. Add the fortrabbit deploy service as a remote (`git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git`)

```shell
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

See the [Statamic Git guide](https://statamic.dev/git-automation#remote-setup) and the [fortrabbit git remote help](/git-deployment#toc-adding-fortrabbit-as-a-remote) as reference.

## 6 - Try it out

Still logged in by SSH on the fortrabbit App, you should be now able to add and commit files with Git to the App and push changes to the deploy service remote from the `editorial` branch.

When pushing for the first time, set the remote as an upstream: `git push -u fortrabbit editorial`.

To get this new branch locally, run `git fetch fortrabbit`.

With the Git repo on your local computer you can fetch and checkout the `editorial` remote branch. Pull changes from there and merge them back into your `main` branch that you are using for development.

## 7 - Git in the Statamic Control Panel

Now you can also use the Statamic Git Automation, depending on the setup, pending editorial changes are visible with the online Git editor in the Statamic control panel or get committed and pushed automatically (we advise using the recommended delay).
