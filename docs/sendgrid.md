---

template:         article
naviTitle:        SendGrid
reviewed:         2019-09-30
title:            Using SendGrid with fortrabbit
group:            Transactional_e-mail
section:          Extending_fortrabbit
stack:            all

websiteLink:      https://sendgrid.com?utm_source=fortrabbit
websiteLinkText:  sendgrid.com
dataCenters:      n/a
image:            sendgrid-mark.svg

keywords:
    - "transactional mail"

---


## About SendGrid

SendGrid offers an e-mail delivery service that assists businesses with transactional e-mail management.


## Pricing

Like other e-mail services, you'll find two different pricing models:

1. Fixed monthly subscriptions
2. Pay as you go by volume
3. There is also a free plan to get started


## Booking & signing up

Choose your desired plan to initiate the sign up. You'll first just need to choose a user name, tell them your email address and choose a password. Later on they want to know a little more about you. "Provisioning" your account includes human! checks.

## Setting it up

In order to get your mails through the SPAM filter of your users, you'll need to authorize the ownership of the domain you are using — think DNS modifications. fortrabbit is not envolved here. You do this with your domain or DNS provider of choice. This is done with DKIM ([SendGrid docs](https://sendgrid.com/docs/Glossary/dkim.html)) and SPF ([SendGrid docs](https://sendgrid.com/docs/Glossary/spf.html)).



## Using SendGrid with fortrabbit

Once you have the setup done, you can finally start using SendGrid from your fortrabbit App. This is straight forward:

The **SendGrid PHP library** [here on GitHub](https://github.com/sendgrid/sendgrid-php) can be required from your `Composer.json` like so:

```
{
  "require": {
    "sendgrid/sendgrid": "~4.0"
  }
}
```

To send e-mails via the API, you'll need to specify your SendGrid API key. We recommend to store this with your [App secrets](/secrets). There is also an [PHP SendGrid example on GitHub](https://github.com/sendgrid/sendgrid-php-example) which makes use of both, the API and the SMTP gateway.
