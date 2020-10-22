---

template:      article
reviewed:      2020-10-19
title:         Firewalling
naviTitle:     Outgoing requests
excerpt:       What you may want to know about making outgoing calls and our blocklist.
lead:          Having trouble making outgoing calls from your App? That's maybe because we block most non-standard ports. You can request to get your opened.
group:         troubleshooting
stack:         all
dontList:      false

---

Outgoing traffic is limited for security reasons — most non-standard ports for making outgoing calls are locked. Only the most standard ports are allowed — see our [specs](http://www.fortrabbit.com/specs#firewall). Please make sure to visit that page before your request to open ports.


## Requesting to open the firewall for certain ports

1. Login to the Dashboard
2. Browse to your App
3. Go to the firewall settings 
4. Hit "request a custom white listing"
5. Fill the form
6. Wait until we reply

<div markdown="1" data-user="known">
[Request a new firewall white-listing for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/firewall)
</div>
