---

template:      article
reviewed:      2020-03-06
title:         Billing on fortrabbit
naviTitle:     Billing
excerpt:       Our consumption based pricing model explained.
group:         platform
stack:         all

---

This article covers the topics like billing, pricing, payments, invoices and taxes. Please read the link:/billing-contact[Billing Contact] article to learn about the practical side of things in our Dashboard.


### Free trial

You can test fortrabbit before paying a single a dime, completely for free. All you need to do is sign up and upload some code. The trial will end automatically, you don't have to cancel it and you will not be charged. No credit card required. Once you decide to book a paid plan, the following sections will apply.


### Consumption based billing

There are no upfront costs or payments. The service is priced by usage, similar to how other cloud providers do it. This may break some expectations, but in practice this is the regular flow;

1. A client signs up and opts in to pay for the service
2. Their App is in use for a few days
3. At the end of the month, the client receives an invoice for the preceding days

German users may recognize this as prorate, just like with the old telephone bill -
at the end of the month, you get an invoice for the consumption during that month.


#### Daily billing cycle

For example, if you order one of our plans on the 16th of a 30-day month, the invoice will be calculated for 15 _active days_ since you started using the service. The minimum billing period is one day;

```
      April 2020               April 2020
 Mo Tu We Th Fr Sa Su     Mo Tu We Th Fr Sa Su
        1  2  3  4  5            -  -  -  -  -
  6  7  8  9 10 11 12      -  -  -  -  -  -  -
 13 14 15 16 17 18 19      -  -  -  $  $  $  $  <- the  16th is on a Thursday
 20 21 22 23 24 25 26      $  $  $  $  $  $  $
 27 28 29 30               $  $  $  $
```

The dollar-sign indicates the days you would be billed for if signe up on the 16th.


### Monthly invoices

Within the first few days of every new month, you will get the invoice for the consumption of the last month. After receiving the invoice, the payments are due and are charged automatically. Credit Cards payments will usually happen within the first few days of the month. SEPA direct payment will be charged within the first two weeks.


#### The monthly billing cycle in detail

The fortrabbit billing bot calculates the due amount for each Billing Contact at the end of the month. Based on that data, a new invoice will be created. The invoices are accessible through our Dashboard. The invoice is dated to the last day of the month and the service period is that month.

* On the first day of the following month, all clients are notified about this new invoice.
* On the 3rd of the month, the fortrabbit billing bot will charge all invoices having credit card as the payment method.
* Around the 7th of the month, all the SEPA direct payments invoices are issued and executed a few days later.

In the middle of a month, a human will look over bounced invoices. At that time, due payments will be re-charged and additional payment reminders may be sent out.


### Costs monitoring

You can check the current costs by looking at your *Billing Contact*, see link:/billing-contact#toc-costs-monitoring[here].


### Payment methods

You can pay by credit card or SEPA direct debit. For enterprise clients (large volume) we also offer to pay by bill (giro / wire) (on account, after invoice).

* Supported credit cards are Visa, MasterCart and Amex.
* SEPA direct debit is accessible to clients in the European Union.
* Debit cards are not supported.
* PayPal is not supported. There are no future plans to integrate with PayPal.


### Billing e-mail address

You can use a different e-mail address for each link:/billing-contact#toc-changing-the-billing-e-mail-address[Billing Contact] in your company.


### Taxes

We consider our primary clients to be entrepreneurs and other businesses. In other words, fortrabbit is primarily a B2B service. While that can mean anything and nothing, it carries some fiscal implications:

* Our prices are shown as net prices without Value Added Tax (VAT).
* If you are a non-VAT registered company in a EU country, we add VAT.
* If you are a VAT-registered company in a EU country and add your valid VATIN, we don't charge you VAT.
* If you are a VAT-registered company in Germany, we add VAT, but you will get it back from your tax-office.
* If you are a company from a NON-EU country, we don't charge you VAT.


### VAT Tax ID (VATIN) from clients in EU countries

We recommend to enter your VATIN. This way you save upfront costs for VAT — reverse charge makes this possible. See this link:/billing-contact#toc-taxes[guide] to setup your VATIN with your Billing Contact.


### Currencies

All link:/company#toc-billing-contacts[Billing Contacts] within the EU are required to pay in EUR. Billing Contacts from other countries can pay in USD or EUR.

It is not possible to change the currency of a Billing Contact later on. However, you can create a new Billing Contact with a different currency and then move Apps there.


### Pending transactions

When you create a Billing Contact, we'll do a debit test on your credit card. This should not show up on your credit card statements, because we will not finalize this trsansaction. Some clients have reported that this transaction shows up in their online banking. In any case, this reserved amount will not be claimed by us.


### Downloading previous invoices

*Option 1*: Search your e-mail inbox for "fortrabbit invoice". Each invoice e-mail contains a secret link to the invoice. From there you can print or download the invoice.

*Option 2*: Use the "Download / print" button from the link:/dashboard[Dashboard]: Login to the Dashboard &gt; Your Account &gt; Companies (Your Company) &gt; Billing Contact &gt; Invoice archive &gt; Invoice &gt; Download &amp; print invoice.

Invoices are shown as HTML. Depending on support in your browser, a PDF can be generated using the "Print to File" feature.


### Bounced payments

Sometimes payments may bounce, that just happens. Please note that our bank
collects fees for failed charges or insufficient funds. We may pass those fees
to you.

There are various reasons why payments bounce:

* Insufficient funds - we will try to debit again after a few days
* Expired credit cards - link:/billing-contact/#toc-changing-the-payment-method[update your payment method]
* Something else - link:billing-contact/#toc-changing-the-payment-method[update your payment method] (enter a new card or switch to SEPA)

Please contact us if you are aware about open invoices with us and or if you have questions about an invoice. In any case, we will inform you by by e-mail about failed payments.


#### Service cancellation after repeatedly bounced payments

At some point, after several unsuccessful attempts to charge your account, your App will be deleted. We will not take your data hostage until you pay us, we will permanently delete Apps. ***Deleted Apps can not be recovered.*** https://blog.fortrabbit.com/bounced-payment[Read why we do this in a story from our blog].

We consider deleting Apps for bounced payments carefully with human review on a per client basis. Here is the basic rule set:

For brand new clients with no previous invoices: In case the very first invoice keeps on bouncing (we will retry) and you have not reacted to our e-mails, we will proceed with the deletion of the App right away.

For clients with less than 6 invoices or when the credit card processor reports possible fraud or the Account looks suspicious: We might delete the App after the first bounced invoice (we will retry) or if the client fails to respond to our e-mails.

For long term clients with more than 6 paid invoices and at least 2 bounced invoices (we will retry) in a row or a total of 4 unpaid invoices: Before deletion we will attempt to write a personal e-mails, beside the automated ones. We will also try to contact everyone involved in the Company or project. We might delete Apps when there was no reply on our attempts to reach out.

In any case, with 3 open invoices in a row and no reply: We will immediately delete Apps.

### Paying open invoices

Open invoices can be found in the Dashboard;

Login to the link:/dashboard[Dashboard] &gt; follow the instructions in the "open invoices" warning.

Credit card payments are done immediately. SEPA direct payments are delayed. If your credit card has insufficient funds at the time of charging, we may try to charge it again during the month.

### No upfront payments

It is not possible to pay upfront because our billing model is based on consumed resources up to present time.

### No yearly payments

Yearly payments are not supported because we do not accept upfront payments because the costs and the exact usage can not be predicted in advance.

### No refunds

We are not offering refunds at this time.

This is our current policy because we have a custom billing system and refunds are complicated to do correctly under German accounting laws. We would have to send you a reduced negative invoice and it would require additional manual steps in book keeping for this.

If you think something is wrong with your invoice, please contat us. We may be able to give you a discount on your next invoice to show good will on our part.

### Account cancellation

You can always quit, scale down or kill certain Apps.

You can also cancel your Account completely. In this case, we will delete all data connected to your account, including all code and database records hosted by us.

The invoices are an exception to this. We are required by law to keep billing related data, that is the invoices and the information contained there. The same applies to your credit card information, which is stored by our credit card service provider.
