---

template:      article
reviewed:      2020-10-29
title:         Firewalling
naviTitle:     Firewalling
excerpt:       What you may want to know about making outgoing requests and our blocklist.
lead:          Having trouble making outgoing requests from your App? That's maybe because we block most non-standard ports. You can ask us to get yours openned.
group:         troubleshooting
stack:         all
dontList:      false

---

Outgoing traffic is limited for security reasons — most non-standard ports for making outgoing requests are locked. Only the most standard ports are allowed — see our [specs](http://www.fortrabbit.com/specs#firewall). Please make sure to visit that page before making your request to open ports.


## Requesting to open the firewall for certain ports

1. Login to the Dashboard
2. Browse to your App
3. Go to the firewall settings 
4. Hit "Request a custom firewall rule"
5. Fill the form
6. Wait until we reply

<div markdown="1" data-user="known">
[Request a new firewall white-listing for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/firewall)
</div>
