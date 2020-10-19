---

template:      article
reviewed:      2020-10-19
title:         503 errors
naviTitle:     503 errors
excerpt:       What you may want to know about our 404 errors.
lead:          Are you seeing a 503, 502 or 500 error page? 
group:         troubleshooting
stack:         all
dontList:      false

---

In most cases, this is NOT a server failure, it's a software problem caused by your configuration. And it's something you can likely solve yourself.

There are many different reasons for 5xx errors. Be a detective and start the investigation by examining the logs, see [here for Uni Apps](logging-uni) and [here for Pro Apps](logging-pro). Within the logs you will find where application exited with which error: This is the hot lead you are after.
