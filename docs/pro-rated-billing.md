---

template:   article
reviewed:   2022-09-01
title:      Pro-rated billing explained
naviTitle:  Pro-rated billing
excerpt:    "How does pro-rated billing work?"
lead:       "There are no upfront costs or payments here. Pay for what you use. This is how it works in detail."
group:      billing
stack:      all
rank:       100

---

## Daily billing cycle

If you order one of our plans on the 16th day of a 30-day month, the invoice will be calculated for 15 active days since you started using the service. The minimum billing period is one day. The dollar signs indicate days that would be included in the invoice:

```
       April
Mo Tu We Th Fr Sa Su
       -  -  -  -  -
 -  -  -  -  -  -  -
 -  -  -  $  $  $  $
 $  $  $  $  $  $  $
 $  $  $  $
```

## No upfront costs

The above example gives you a clue that we are charging after usage. First you use the service, at the end of the service period, actual costs are calculated together and invoiced. That means:

* You don't pay on booking, only after the first month of usage
* After deleting an App, it will appear on the next invoice for the last service period this was in use

### Monthly prices are shown

Monthly prices are shown on our pricing pages and when booking something in our Dashboard. This is because the billing cycle is monthly and most Apps stay for many months. You can see the daily prices with our [specs pages](https://www.fortrabbit.com/specs).

### Daily prices on the specs page are based on 30 days

To make things easier we divide the monthly price by 30 to show the daily price.

* $0.5 is the daily price for a Component that costs $15 monthly

This is easy math. However, not all months have 30 days and by design we have fixed monthly prices. That's why in reality the daily prices are dependant on the number of days a month has. Here is the example with a $15 monthly price:

* January: 31 days = $0.4838709677 daily price
* February: 28/29 days = $0.5357142857/$0.5172413793 daily price
* March: 31 days = $0.4838709677 daily price
* April: 30 days = $0.5 daily price
* May: 31 days = $0.4838709677 daily price
* June: 30 days = $0.5 daily price
* July: 31 days = $0.4838709677 daily price
* August: 31 days = $0.4838709677 daily price
* September: 30 days = $0.5 daily price
* October: 31 days = $0.4838709677 daily price
* November: 30 days = $0.5 daily price
* December: 31 days = $0.4838709677 daily price

When you multiple the daily price by the number of days in the month you will end up roughly with a fixed monthly price of $15. On average months have 30.437 days. That means the 30 days based daily price is a bit higher than the actual price to pay.

## Monthly invoices

Within the first few days of every new month, you will get the invoice for the consumption of the last month. After receiving the invoice, the payments are due and are charged automatically. Credit card payments will usually happen within the first few days of the month. SEPA direct payments will be charged within the first two weeks.

### The monthly billing cycle in detail

* End of the month: Invoices for the monthly usage are created 
* 1st of the month: Invoices for the previous month are sent out
* 3rd of the month: Credit card payments are issued
* 7th of the month: SEPA direct payments are issued
* Until end of the month: Review and retry of bounced invoices

## No upfront payments

It is not possible to pay upfront because our billing model is based on consumed resources up to present time.

## No yearly payments

Yearly payments are not supported because we do not accept upfront payments because the costs and the exact usage can not be predicted in advance.
