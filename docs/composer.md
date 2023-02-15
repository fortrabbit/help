---

template:      article
reviewed:      2023-02-15 08:20:00
title:         Leveraging Composer
naviTitle:     Composer
lead:          Composer is the defacto standard to handle PHP application dependencies, as well as providing mechanisms to keep them up-2-date. Learn how to integrate Composer into your development workflow with fortrabbit.
group:         development
stack:         all

keywords:
  - composerphp
  - git
  - getcomposer
  - Dependency Manager

---


## Requirements

It's recommended that you have Composer running in your [local development environment](/local-development) as well.


## Composer with Git

```
Local         fortrabbit deploy service           Your fortrabbit App
┌────────┐    ┌────────────┐  ┌────────────┐      ┌──────────────┐
│        │    │            │  │            │      │              │
│  Git   ├────▶  Git repo  ├──▶  Composer  ├──────▶   webspace   │
│        │    │            │  │            │      │              │
└────────┘    └────────────┘  └────────────┘      └──────────────┘
```

The above drawing illustrates the fortrabbit architecture. When you deploy with Git, Composer will run **automatically** along within the deployment process on fortrabbit.  So, when you `git push`: 

1. the Git repo will be updated
2. a `composer install` will be triggered
3. new files will be distributed to the webspace

## Advanced usage

### The vendor folder

The `vendor` folder should NOT be in Git: Make sure that folder is included in your `.gitignore` file. This directory is created by Composer within your project locally and contains all the packages you are using locally. Make sure that both the `composer.lock` file and the `composer.json` file are present and part of Git.

### Composer in the deployment file

You can fine-tune your deployment behavior and aspects of Composer in the deployment file. See the options under the `composer:` keyword [here](deployment-file-v2).

### Alternative locations

If your `composer.json` and `composer.lock` files are not on the top level, then they will be ignored by the deployment. However, you can use the `pre` directive from the [deployment file](deployment-file) to set up a custom composer run in a different directory.

Here is an example which assumes that your `composer.*` files are within the folder `sub-folder`:

**fortrabbit.yml**

```yaml
version: 2

# execute alternate composer run before anything
pre: /usr/local/bin/composer install --prefer-dist -d sub-folder

# make sure the new vendor folder is sustained during deploys
sustained:
  - sub-folder/vendor
```

### Private repositories in Composer

You need to add the private repositories into your `composer.json` file? Read on [here](/composer-auth.md).

## Composer from SSH

[Universal Apps](/app-uni) only: While we highly recommend leveraging Composer via the Git deployment (see above), you can also execute Composer when logged in via [SSH](ssh-uni). Please note that this should only be done in certain edge cases.

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
$ composer install
```

This way Composer will be executed within your App's web delivery environment, which is not optimized for such tasks. Please don't use `composer update` as this might cause Composer to hit the App's memory limits.

## Troubleshooting

### Memory problems with Composer

The most common Composer issue is, when users are running into memory problems when trying to execute `composer install` while logged in via SSH here. The general answer on that is: Please don't. Use Composer together with Git deployment as described above. 
