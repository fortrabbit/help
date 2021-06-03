---

template:         article
reviewed:         2021-06-03
naviTitle:        GitHub
title:            Combine fortrabbit with GitHub
lead:             Learn how to integrate the most popular Git-as-a-service provider with your fortrabbit workflow.
stack:            all

group:            Code_collaboration
section:          Extending_fortrabbit

websiteLink:      https://github.com?utm_source=fortrabbit
websiteLinkText:  github.com
image:            github-octocat.png
dataCenters:      n/a

keywords:
    - git-hub
    - addon
    - add-on
    - integrations
    - integration
    - gitlab
    - bitbucket
    - coding
    - API
    - CI
    - cloud
    - vcs
    - add-on
    - version control

---

## About GitHub

You hopefully already know that [GitHub](https://github.com) is the most popular choice for versionized code hosting and collaborating. It is free to use with open source projects. This means that all the code you publish in the free version will be visible to everyone. But you can — of course — [purchase a plan](https://github.com/pricing) including private repos as well. Similar alternatives are Bitbucket and GitLab (and Coding if you are from China). They basically work the same, so most concepts of this guide apply as well.

### GitHub is not Git

GitHub is so popular that beginners sometimes confuse GitHub with Git. Git is the version control system established by Linus Torvalds. GitHub is the service, which offers Git remote hosting and additional extra magic collaboration features. If you are new to Git, check out our [getting started with Git article](/git).

## Why to combine GitHub with fortrabbit

Your fortrabbit Apps are already coming with a [Git repo](/git-deployment). So why to combine GitHub with fortrabbit at all? The fortrabbit Git repo is vanilla plain, GitHub and alike are offering enhanced pull request workflows, through their web interfaces. So in general, it's for professionally managed projects with three or more team members.

## Simple integration

This example shows you how to combine the two services in an easy detached way. The idea is, that the code development master is hosted on GitHub while each developer has it's [own local dev env](/local-development). fortrabbit is the live production. Once changes, or a new version is ready, it get's deployed to fortrabbit. All developers have access to GitHub, while for example only the lead developer also has access to fortrabbit. There hir needs to be able to push the code base either to fortrabbit or to GitHub.

Add your external Git-provider as one remote and fortrabbit as another remote. Then you can push to your external provider while in development and collaborating; push to fortrabbit to see the code in action. Here is how you add your App's Git remote on fortrabbit to your already existing local working copy of a GitHub / Bitbucket / GitLab repo and then work with it:

```bash
# add your App's remote and name it "fortrabbit"
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# now push to the master branch of your App's remote on fortrabbit
$ git push fortrabbit master
```

## GitHub Actions

GitHub is also offering [Github Actions](https://github.com/features/actions), an integrated continuous integration solution. Combining GitHub Actions with fortrabbit can help to integrate testing and frontend build systems based on Node.JS (think Webpack). We do not offer predefined GitHub Actions yet, but [this blog post](https://blog.fortrabbit.com/how-to-use-github-actions) contains two templates to get started.

## GitHub API limits and Composer

The reason why you can run into this issue temporarily is that GitHub limits it's API per IP. Given that you deploy on our Git servers (or use composer from our SSH servers) this IP address is shared with other developers deploying there as well. So if there is a deployment spike, GitHub might close down for a while.

The solution is to create a GitHub OAuth token and put it in your `composer.json` file. You can do so by visiting [https://github.com/settings/tokens](https://github.com/settings/tokens) or via terminal:

```bash
curl -u your-github-user -d '{"note": "Fortrabbit Auth"}' https://api.github.com/authorizations
```

If you are using multi factor authentication (OTP), then you need to provide a valid OTP token like so:

```bash
curl -u your-github-user -H 'X-GitHub-OTP: 123456' -d '{"note": "Fortrabbit Auth"}' https://api.github.com/authorizations
```

This will give you a response like the following:

```
{
  "id": 123456,
  "url": "https://api.github.com/authorizations/123456",
  "app": {
    "name": "Fortrabbit Auth (API)",
    "url": "http://developer.github.com/v3/oauth/#oauth-authorizations-api",
    "client_id": "12345abc12345"
  },
  "token": "12345abc1234512345",
  "note": "Fortrabbit Auth",
  "note_url": null,
  "created_at": "2013-08-08T11:08:18Z",
  "updated_at": "2013-08-08T11:08:18Z",
  "scopes": []
}
```

What you need is the `token` value. In the above example it is `12345abc1234512345`. Open up your `composer.json` file and add the token withunder the `config` directive:

```
{
  "require": {
    "awesome/package": "@dev"
  },
  "config": {
    "github-oauth": {
      "github.com": "12345abc1234512345"
    }
  }
}
```

That's it. Your API token will be used to give you a personal rate limit which is much more higher than the default one.
