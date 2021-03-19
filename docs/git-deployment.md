---

template:      article
reviewed:      2019-07-28
naviTitle:     Git deployment
title:         Deploy with Git on fortrabbit
lead:          Learn how to get your code up and running with a simple git push.
group:         deployment
stack:         all

keywords:
    - beginner
    - dashboard
    - deployment

---

## Git ready

We assume that you have: [Git installed](git) locally and know the basics. We further assume that you know about [access methods](/access-methods) here on fortrabbit, so either have your SSH keys installed or your [Dashboard](/dashboard) password handy.

## Usage

Each fortrabbit App comes with its own Git repo. Note that this repo is not located on the App's web storage itself, but on a separate deployment Node. Set your App's Git URL as a Git remote in your local Git working copy. To deploy just push your code to that remote's master branch. 

### Simple Git deployment workflow

1. Clone the (empty) repostory
2. Go to the app-name folder
3. Do stuff
4. Initialize Git locally
5. Set upstream and 1st push
6. Every deploy from now on

On the terminal this will look similar to this:

    git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
    cd {{app-name}}
    echo '<?php echo "PHPower to the PHPeople";' >index.php
    git add index.php
    git commit -am 'Initial commit'
    git push -u origin master
    git push

After the first deployment is done, the traced files from te Git repo will be synced into the App's web space so that you can worship your work in the browser: [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

Next time you want to send changes to your App, you will have to add new or changed files and subequently do a git-push:

    $ git add new-file.txt
    $ git add index.php
    $ git commit -m 'new version'
    $ git push

### Adding fortrabbit as a remote

```
# Using Git already? Add fortrabbit as an additional remote:
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
```

### Resetting the remote repo

To start with a complete new Git history, you can now reset your repository. This can be done with the `reset` command like so:

```
# Reset the remote repo (delete remote Git repo & vendor folder):
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com reset
```

The reset operation is non-destructive, meaning: It does not generate a release. Thereby your live App continues to operate without any interruption. The new release will only be built on the next push (of your new code base). A repository reset also removes any sustained directory (the `vendor` folder, so a following [Composer](composer) install requires to download everything once again).


### Git with a GUI or IDE

You can also use a graphical interface like SourceTree, Tower and so on - see the [official list of Git GUIs](https://git-scm.com/downloads/guis) – or an IDE like PhpStorm or Eclipse to manage Git. You'll need these access credentials:

* **SSH clone URL**: {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}
* **SSH password**: {{ssh-password}}

## Advanced usage

Still reading? Dig deeper!


### Git deployment vs Universal Apps

Universal Apps have persistent storage, which you can access via [SSH](ssh-uni) or [SFTP](sftp-uni). It further means that runtime data, like user uploads, are persistent and *will not be removed* upon Git push.

To make sure nothing is deleted, all git deployments to Universal Apps follow an **overwrite but not delete** strategy which is thoroughly explained in the [deployment methods article](deployment-methods-uni#toc-git-push-overwrite-but-not-deletes).

### Behind the scenes

```nohighlight
┌─────────┐             ┌──────────┐             ┌───────────┐
│         │             │          │             │           │
│   You   ├──Git push───▶ Git repo ├───deploy────▶    App    │
│         │             │          │             │           │
└─────────┘             └──────────┘             └───────────┘
```

Your `git push` updates the Git remote on fortrabbit and triggers the build of a new release package. This new release package will then be distributed to all the Nodes your App runs on. All files on the Nodes will then be replaced by the ones contained in the release package. Check out [this video](deployment-architecture-video) to understand the flow.


### The branch name counts

While you can have as many Git branches you want, only changes in certain branches will be deployed. The `master` branch is the default one.


### Branching for multi-staging setups

Usually, only the remote Git `master` branch will be deployed. You can also create a branch with the same name as your App, which will be preferred over the master branch. That way it is easier to set up a [multi-staging environment](multi-staging).


### Deployment file

Fine tune deployment configurations with the `fortrabbit.yml` deployment file: control the way Composer runs, define pre- & post-deploy scripts and more. You can find a full example [here](deployment-file-v2).

### Composer

[Composer](composer) install will always run after a successful `git push` - as long as both `composer.json` and `composer.lock` are present. Read on [here](composer).

### Private Git/Composer repos

You can also include your own private Composer repository as described [here](private-composer-repos).

### Large Git repos

A bloated Git repository slows down deployment. You might even hit our limits. In most cases the repo can and should be much smaller.

#### Large files

We don't actively enforce a single file limit, but you should not put big binary files (> 2 MB) into your Git repo.

In general we also advise not to put assets and most importantly images or even videos in Git. Some images that belong to your website layout, like your company logo (a small SVG) are ok. But when you have a lot of images in Git, odds are that this is bad practice.

For almost all cases, your uploaded content images (`.jpg`, `.png`, `.gif`) do not belong in Git. Remember that the Git repo and the web-space are different things here at fortrabbit, [Git is a one-way street here](/deployment-methods-uni#toc-git-works-only-one-way). The next user upload will not be in Git anyways. It's a good practice to **separate code from content** for many reasons. So we advise to deploy user uploads, static assets and other runtime data separate from the core code deployment done with Git. With Universal Stack you can use SFTP/SSH or rsync for this. With the Professional Stack you'll manage those assets on the [Object Storage](/object-storage) anyways.

#### The hidden .git folder

Git never forgets. It can bring back any content from any file, even deleted ones. To do so, it stores all the stuff in the hidden `.git` folder at the top level of the repo. Over time that file can get big too. It can also contain unreachable blobs and other stuff you are not aware of. Depending on the situation, there are many ways to clean this up: delete files of a certain type, delete the history before a certain date, or even start again from the current state.


### Deployment release package

The release package is a gzipped archive which contains: the App's Git repository + vendor folder + all files generated by pre- or post-deployment scripts. In short: compressed size of ``~/htdocs``.

To keep deployment fast for everyone the size of the release package is [limited](http://www.fortrabbit.com/specs#limits). You can find out the release package size locally by packaging the local copy of your App, including the `vendor` folder and anything else generated by your build scripts:

```bash
$ tar -zcf my-project.tar.gz {{app-name}}
$ ls -lh {{app-name}}.tar.gz
```

### Code revision file

In your deployment there is a hidden file called `.code-revision`. It's located at `/srv/app/{{app-name}}/.code-revision`. and is getting renewed each time you deploy with Git. It includes the Unix Timestamp of the last Git deployment and the hash of the latest commit, separated by a dot. This what the content looks like:

```
1540931024080267920.26b284844c746f80f42ad7ac77a4ad42d25b27de
```

There are various advanced use cases. Hook that file into your deployment cycle to:

* check whether the correct commit has been deployed
* use the timestamp to bust static assets like JS and CSS

<!--
TODO:

* provide usage code example! (see below)
* provide more use cases!

Q: Position to file in ENV var? Or at least concat APPNAME ENV var with location.

```php
<?php
if (getenv('APP_NAME')) {
 $rev    = file_get_contents(sprintf("/srv/app/%s/.code-revision", getenv('APP_NAME')));
 [$ts, $hash] = explode('.', $rev, 2); 

 echo $ts;
 echo $hash;
}
```
-->


### Integrating with GitHub & Bitbucket

We don't have any fancy GitHub/Bitbucket integrations (yet). But it is easily possible to combine your fortrabbit repo with [GitHub](github) and [Bitbucket](bitbucket).


## Git submodules

Git submodules are NOT supported. We recommend to use Git subtrees instead. See [this post from Atlassian](http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/).


