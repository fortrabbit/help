---

template:      article
reviewed:      2023-01-19 14:15:17
naviTitle:     PHP processes
title:         What are PHP processes?
lead:          'This is what you need to know about PHP processes and our implementation.'
group:         platform
stack:         all
hideExamples:  yes
workInProgress: yes

---

## About PHP processes

Unlike fronted Javascript, which is running in the web browser, PHP scripts are being executed on the web server and the output is then usually delivered to the web page as HTML.

A FPM PHP process is a reserved persistent computing resource, ready to be used at any time. Each PHP process can respond to one concurrent PHP request at a time.

## Our implementation

fortrabbit utilizes FPM (FastCGI Process Manager). Beside the allocated PHP memory we spawn a number of persistent FPM PHP processes with each scaling (hosting plan).

- See [Specs Uni Apps](https://www.fortrabbit.com/specs) for actual numbers
- See [Specs Pro Apps](https://www.fortrabbit.com/specs-pro) for actual numbers

## PHP processes, PHP requests and PHP response time

- By design PHP requests should take only a very short time to execute.
  - If it takes longer, it’s likely your code. Refactor it for performance!
- Aim for 250 ms or less on average.
  - Our Dashboard shows you PHP response time metrics.
- A new incoming PHP request will have to wait when all PHP requests are busy.

### A simple example

- Your App has 2 PHP processes
- 1 PHP request takes 500 ms to execute (PHP response time)
- So each PHP process can handle 2 PHP requests per second
- Since there are 2 PHP processes, the App can deliver 4 PHP requests in 1 second

### A bad example

- Your App has 4 PHP processes
- 1 PHP request commonly takes 1000 ms (1 second) - which is too long
- 1 page view of your website is creating 3 PHP requests
- 3 website visitors at the very same time can bring the website down

## PHP processes VS page views

Page views refer to a single request made by a user to view a page on a website. This request will likely involve a number of HTTP requests, such as images, HTML, CSS, and Javascript files, all of which must be downloaded from the Apache web server. 

A page view likely will also require the server to execute any PHP scripts associated with the page, which translates into one PHP process for each PHP script.

For many websites the ratio between page view and PHP request is one to one. But often one page view will generate multiple PHP requests, for instance by making different AJAX calls to the backend to query the database or returning API requests or transforming images.

## PHP processes VS max execution time

Sometimes clients are asking us in support to increase the `max_execution_time`. While this might help to execute long-running tasks, this also blocks the PHP processes for a longer time. See the examples above.

That's we usually advise to lower the time it takes to execute PHP to be able to serve your website to your visitors simultaneously.

Our Pro Stack has [Workers](/workers) to offload long-running background tasks.