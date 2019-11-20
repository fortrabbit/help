---

template:      article
reviewed:      2019-09-30
title:         Billing Contact
naviTitle:     Billing Contact
excerpt:       What you can do with a Billing Contact
lead:          You want certain Apps to be charged from a different credit card while keeping team and access settings.
group:         platform
stack:         all

keywords:
    - beginner
    - platform
    - teamwork
    - sharing
    - roles
    - organization
    - access
    - tenant
    - client
    - employees

---

A Billing Contact represents the payment details which are used by fortrabbit to bill your [Company](/company).

Most people are happy with just one Billing Contact, but in some cases you need one App to be paid by one credit card and another App to be paid by another credit card. Billing Contacts allow just that. A Billing Contact persists of:

* A [payment method](#toc-changing-the-payment-method), which can be credit card or SEPA direct debit (EU only)
* A [invoice address](#toc-changing-the-invoice-address), which will be written on the invoice
* A [billing e-mail address](#toc-changing-the-billing-e-mail-address), to which the invoices will be send to

Each App (except trial Apps) has is always associated with a specific Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing article](/billing).

## Differences between Billing Contact, Company and Account

```
┌───────────────────────────────────┐     ┌─────────────┐
│                                   │ ┌───▶    App 1    │
│             ┌───────────────────┐ │ │   └─────────────┘
│             │ Billing Contact 1 ├─┼─┤   ┌─────────────┐
│             └───────────────────┘ │ └───▶    App 2    │
│   Company   ┌───────────────────┐ │     └─────────────┘
│             │ Billing Contact 2 ├─┼─┐   ┌─────────────┐
│             └───────────────────┘ │ └───▶    App 3    │
│                                   │     └─────────────┘
└───────────────────────────────────┘  
┌─────────────┐    ┌─────────────┐     
│  Account 1  │    │  Account 2  │     
└─────────────┘    └─────────────┘     
```

The Billing Contact is part of a [Company](/company) on fortrabbit. Within the fortrabbit Company you can create multiple Billing Contacts and then assign individual Apps to either Billing Contacts. You could also create multiple Companies to achieve that, but with multiple you can still manage the same team on the Company. Remember: [Accounts](/account) are representing persons here on fortrabbit.

### Creating a Billing Contact

Each Company comes with one Billing Contact out of the box, when creating a Company, you also create a Billing Contact. For most cases this is enough. But in some cases you might want to separate billing within one Company. So you can create multiple Billing Contact under one Company like so:

* Dashboard Home >
* Companies >
* {{ Your Company }} >
* Hit the **New Billing Contact** button

Follow the steps. You will be asked for an invoice address and a payment method. This will give you an empty Billing Contact, no costs, as long as you don't assign an App to the Billing Contact. You must be Owner of the Company for this. You might need to book a Company Plan to enable more Billing Contacts.


### Deleting a Billing Contact

You cannot, because they are bound to previously created invoices and are needed for our accounting. You can, of course, delete or move all Apps away from a Billing Contact so that it becomes inactive and no more bills will be created for it. As the Billing Contact also contains an archive of previously issued invoices, you can always access your old invoices.

To completely delete all invoices and all associated Apps, you can also [delete the Company](/company#toc-deleting-a-company).

### Changing the billing e-mail address

Each month we'll send an invoice and send a link by e-mail. You can change this e-mail to go directly to your book keeping department.

* Within the Dashboard (Home) >
* Companies (Your Company) >
* Billing Contact >
* **Invoice e-mail**

### Changing the invoice address

Your invoices contain the street address of your organization. You will be asked for that while setting up a Billing Contact. You can change this invoice address for future invoices at any time like so:

* Dashboard > Companies > {{ Your Company }} >
* Billing Contact >
* **Invoice address**

### Changing the payment method

A valid payment method is required to use the fortrabbit services. Each Billing Contact has an associated payment method. When creating a Company you can choose between different [payment method options](/billing#toc-payment-methods) and enter your credentials. fortrabbit will charge on a monthly basis [after usage](/billing#toc-consumption-based-billing). Change the payment method of a Billing Contact like so:

1. Dashboard > Companies > {{ Your Company }} >
2. Billing Contact >
3. **Payment method**

Changing the payment method is required when your credit card expired or you which to switch from one payment option to another. Changing the payment method will affect future payments and will also be used to balance possible open invoices.

### Adding a VAT IN

It is recommended to add your VAT IN to your Billing Contact when your company is registered in the European Union. This can save you (and us) upfront value added taxes. Please see our [tax topic](/billing#toc-taxes). To enter your VAT IN:

1. Dashboard > Companies > {{ Your Company }} >
2. Billing Contact >
3. **VAT IN**

Please mind that we need to validate the VAT IN. So the number you enter needs to match the correct format. Please mind that your VAT IN is not the tax number. The correct number should start with your country code. Our VAT IN looks like so:

Entering the VAT IN will change how VAT will be charged for future invoices, entering the VAT IN can not affect past invoices.


### Costs monitoring

You can see a live preview of the usage for the current month: 

1. Log in to the Dashboard
2. Go to your Company (Company Name)
3. Under Billing Contact select the "Current costs"

There you'll find a cost preview (invoice draft) for the ongoing month, covering all of your Apps. This shows what will be printed on the next invoice. Please mind that it will show the current costs of the day of the month so far, it will show different values the next day.


### Changing the Billing Contact of an App

In case you want the App to be billed from another credit card or bank account but still want to keep the team of the App the same: You simply change the Billing Contact of the App. You first need to have at least two Billing Contacts with your Company. Then, please see the instructions with [app article](app#toc-changing-app-ownership) on how to change ownership of an App.
