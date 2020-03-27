---

template:      article
reviewed:      2020-03-06
title:         Billing on fortrabbit
naviTitle:     Billing
excerpt:       Our consumption based pricing model explained.
group:         platform
stack:         all

---

This article is all about the concepts of billing, pricing, payments, invoices & taxes. Please also check out the [Billing Contact](/billing-contact) article to learn about practical usage in the Dashboard.


### Free trial

You can test fortrabbit before you pay a dime. All you need to do is: sign up to fortrabbit and try it for free. The trial will end automatically, you don't have to cancel it. No credit card required. Once you decide to book a paid plan the following applies:


### Consumption based billing

Pay after usage: No upfront costs nor payment! **First you book, then you'll use it, then we'll invoice you** for what you have actually used. It's prorate, just like with your old telephone bill. At the end of the month you'll get an invoice for what you have used in that month. 


### Daily billing cycle

When you book something on the 16th day of a month and the month has 30 days you only pay for the 15 days you're using the product. The minimum billing period is one day. 


### Monthly invoices

Within the first days of every new month you will get the invoice for your last month's consumption. After receiving the invoice, the payments are due and will automatically be done by us. Credit Cards usually will happen within the first days of the month, SEPA direct within the first two weeks.


### The monthly billing cycle in detail

At the end of the month, the fortrabbit billing bot will go through all the Billing Contacts and check for the currents month usage. Based on that data, the monthly invoices will be created and made accessible from the Dashboard. The invoice is dated on the last day of the month and the service period is that month. On the first of the (next) month, the fortrabbit mail bot will send an invoice notice by e-mail to all clients. On the 3rd of the month, the fortrabbit billing bot will attempt to charge for all invoice with payment method credit card. Around the 7th of the month the SEPA direct payments for the invoices, for clients with that payment method will be issued and a few days later executed.

At the middle of the month, a human will look after all the bounced invoices. That human also will attempt to issue payment again and will also might write addition payment reminders.


### Costs monitoring

You can check the current costs with your Billing Contact, see [here](/billing-contact#toc-costs-monitoring).


### Payment methods

You can pay by **credit card** (Visa, Master, Amex, no debit cards) or **SEPA direct debit** (EU clients only). For "enterprise clients" (large volume) we also offer to pay by **bill** (on account, after invoice).


### Billing e-mail address

You can setup a different e-mail address for each [Billing Contact](/billing-contact#toc-changing-the-billing-e-mail-address).


### Taxes

fortrabbit is a B2B hosting solution, it's for entrepreneurs only, we expect you to use it professionally. While that can mean anything and nothing it has some fiscal implications:

* Our prices are shown as net prices without Value Added Tax (VAT).
* If you are a non-VAT registered company in a EU country, we add VAT.
* If you are a VAT-registered company in a EU country and add your valid VAT IN, we don't charge you VAT.
* If you are a VAT-registered company in Germany, we add VAT, but you will get it back from your tax-office.
* If you are a company from a NON-EU country, we don't charge you VAT.


### VAT-IN from clients in EU countries

We recommend to enter your VAT-IN. This way you save upfront costs for VAT — reverse charge makes this possible. See this [guide](/billing-contact#toc-taxes) to setup your VAT IN with your Billing Contact. 


### Currencies

All [Billing Contacts](/company#toc-billing-contacts) within the EU are required to pay in EUR. Billing Contacts from other countries can choose rather they like to be charged in USD or EUR. It is not possible to change the currency of a Billing Contact later on. But you can: create a new Billing Contact with a different currency and then move Apps there. Please mind that customers from the EU have to pay in EUR.


### Pending transaction

When you create a Billing Contact, we'll do a debit test on your credit card, when you enter the informations with us. This should not show up on your credit card statements, as it just a test. We have heard from clients that it sometimes shows up. Please don't worry, it will not be processed.


### Downloading previous invoices

**Option 1**: Scan your e-mail inbox for mails with the subject "fortrabbit invoice" for the monthly invoices we have sent you. Each e-mail contains a secret link to the invoice. From there you can print/download.

**Option 2**: "Download/print" invoices from the [Dashboard](/dashboard): Login to the Dashboard > Your Account > Companies (Your Company) > Billing Contact > Invoice archive > Invoice > print.

Invoices are shown as HTML > you can use print from the browser to generate and download a nicely styled PDF.


### Bounced payments

Sometimes payments bounce, that just happens. Please mind that banks collect fees for those non-sufficient funds. We might pass those fees to you. There are various reasons why payments bounce: 

* Insufficient funds - we will try to debit again after a few days, best please contact us
* Expired credit cards — [update your payment method](/billing-contact/#toc-changing-the-payment-method)
* Something else - [update your payment method](billing-contact/#toc-changing-the-payment-method) (enter a new card or switch to SEPA) or contact us

In any case, we will inform by e-mail when a payment didn't succeeded. Please contact us when you are aware that you have open invoices with us and/or questions about it.

#### Service cancellation after bounced payments

At some point we have to delete your Apps to avoid further costs on both sides. Please note that we will not take your data hostage until you pay us. We will actually delete Apps. Deleted Apps can not be recovered by us. [Read why we do this in a story from our blog](https://blog.fortrabbit.com/bounced-payment).

We consider deleting Apps for bounced payments carefully with human review on a per client basis. Here is the basic rule set:

For brand new clients with no previous invoices: In case the first ever invoice keeps on bouncing (we will retry at least 2 times over  a week) and you have not reacted on our mails, we will proceed to delete Apps right away.

For clients with less than 6 invoices or when the credit card processor reports possible fraud or the Account looks suspicious: We might delete Apps about after the first bounced invoice (we will retry at least 2 times) and no further reply on our mails.

For long term clients with more than 6 paid invoices and now at least 2 bounced invoices in a row or a total of 4 unpaid invoices: Before deletion we will attempt to write personal e-mails. We will try to contact everyone involved in the Company or project.

In any case, with 3 open invoices in a row and no reply from the client: We will delete Apps. If possible, we will try to delete uncritical Apps first:  We will look of there is an active domain routed, or if there is traffic, even if the website looks finished and if there has been any recent Git commits.







### Paying open invoices

You pay your open invoices in the Dashboard yourself like so: 

Login to the Dashboard > follow the instructions on the "open invoices" warning. 

Credit card payments are done "live". SEPA direct payments are delayed. When your credit card didn't had enough balance at first, we might try again during the month.


### No upfront payments

As the billing is based on consumption, we can not foresee what you are about to use. That's why it is also not possible to pay upfront.


### No yearly payments

Yearly payments are not supported, as there are no upfront costs and the exact usage is unknown in advance. Therefore it is also not possible to pay with PayPal.


### No refunds

Sorry, we are not offering refunds at this time. This is because we have a custom billing system and refunds are complicated to do correctly under German accounting laws. We would have to send you a reduced negative invoice and it would require additional manual steps in book keeping for this.

If you think something is wrong with your invoice, contact us anyways. We might be able to give you a "good will reduction" on your next invoice.


### Account cancellation

**You can always quit**. Scale down or kill certain Apps or even cancel your Account completely.

We will delete all your Account data. But it is required by law to keep your billing related data, that is basically the invoices and infos around it. The same applies to your credit card informations, but those are stored with our credit card service vendor — not with us.
