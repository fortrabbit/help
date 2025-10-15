---

template:      article
reviewed:      2025-09-15 08:20:13
title:         Using dig to check DNS settings
naviTitle:     DNS checking with dig
excerpt:       Easy query domain results from the terminal
lead:          'You can use a browser-based DNS lookup tool, but you can also use the terminal to see the current DNS settings of your domain.'
group:         dns
order:         7
stack:         all
dontList:      false

---

With the `dig` command you can see if there are any CNAME entries and where they are pointing to. Here we look up `help.fortrabbit.com` and see a CNAME pointing to the App URL like `help-frbit.frb.io`.

```bash
$ dig help.fortrabbit.com
;; Truncated, retrying in TCP mode.

; <<>> DiG 9.8.3-P1 <<>> ANY help.fortrabbit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44565
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;help.fortrabbit.com.       IN  ANY

;; ANSWER SECTION:
help.fortrabbit.com.      600    IN  CNAME   help-frbit.frb.io.
help-frbit.frb.io.        300    IN  CNAME   help-frbit.eu2.frbit.net.
help-frbit.eu2.frbit.net.  20    IN  A       52.48.51.144

;; Query time: 863 msec
;; SERVER: 192.168.178.1#53(192.168.178.1)
;; WHEN: Wed Jun  1 10:25:40 2016
;; MSG SIZE  rcvd: 122
```

### Dig an IP

```bash
# This will print out the IP of your App
$ dig +short {{app-name}}.frb.io 
```

See also [here](/quirks#toc-outgoing-ip-address) for why you'll probably need your App's IP.
