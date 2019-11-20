---

template:       article
reviewed:       2019-09-27
naviTitle:      Bitbucket
title:          Combine fortrabbit with Bitbucket
lead:           Learn how to integrate the second most popular Git-as-a-service provider with your fortrabbit workflow.
stack:          all
group:          Code_collaboration
section:        Expanding_fortrabbit
workInProgress: yes

websiteLink:      https://bitbucket.org/?utm_source=fortrabbit
websiteLinkText:  bitbucket.org
category:         Code collaboration
dataCenters:      n/a
image:            bitbucket-mark.svg

keywords:
    - addon
    - add-on
    - integrations
    - integration
    - API
    - CI
    - cloud

---

[Bitbucket](https://bitbucket.org?utm_source=fortrabbit) is a Git hosting service that offers advanced Git work-flows, such as 'pull requests'. It offers private repos in a free plan and is a popular choice in combination with fortrabbit. It also offers [paid plans](https://bitbucket.org/product/pricing) for "growing teams".

Bitbucket is similar to GitHub, please hop over to the [GitHub integration](github) article to learn about available work-flows, like having two remotes.

## Deployment pipeline

Bitbucket has pipelines to handle hooks to build stuff. In order for BitBucket to push to your fortrabbit App, you have to [create an SSH key pair with Bitbucket](https://confluence.atlassian.com/bitbucket/use-ssh-keys-in-bitbucket-pipelines-847452940.html) and then [add the public key to your fortrabbit App](access-methods#toc-app-only-ssh-keys).

### Example pipeline

The easiest way to deploy to fortrabbit from Bitbucket is to `git push` directly to our deploy service in your pipeline, add this to your `bitbucket-pipelines.yml`:

```yml
pipelines:
  branches:
    master:
      -   step:
            name: Deploy to STAGING
            deployment: staging
            script:
              - git push --force {staging-app-name}@deploy.{region}.frbit.com:{staging-app-name}.git HEAD:master
      -   step:
            name: Deploy to PRODUCTION
            deployment: production
            trigger: manual
            script:
              - git push --force {production-app-name}@deploy.{region}.frbit.com:{production-app-name}.git HEAD:master
```

Note: This simple workflow does not work if you are building assets in your pipeline that end up adding or changing files that you want to deploy. For that you will need to do a `git commit` in your pipeline and that is out of scope for this simple guide.

For more information about how you can manage multiple environments like staging and production on our platform see our [multi staging article](/multi-staging).
