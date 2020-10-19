---

template:      article
reviewed:      2020-10-19
title:         Denylist
naviTitle:     Denylist
excerpt:       What you may want to know about our blocklist.
lead:          Can not make outgoing calls from your App? We block a lot of ports. 
group:         troubleshooting
stack:         all
dontList:      false

---

We are actively filtering deployment traffic for security reasons: too many falsy login attempts or parallel connections are considered dangerous and will therefore get blacklisted. This applies to all kind of deployment connections, S3, SSH/SFTP and Git.

Before thinking about blocklisting, please assure that it has worked before. We haven't had any support case where blocklisting occurred before it worked at least once. So, when you have not been able to connect before, chances are very very high that this is NOT blacklisting. Please see our [access methods troubleshooting section](/access-methods#toc-troubleshooting) next instead.

Get a new IP by disconnecting from the internet shortly, by restarting your router device. <a class="type-bold" href="#asd" onclick="Intercom('showNewMessage', 'I might have been blocklisted, my IP is: __.__.__.__. My App Name is _______. Its not the first time I am trying to connect, it has worked before.')">Ask us</a> to remove your IP from the blacklisting ban.