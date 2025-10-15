---

template:      article
reviewed:      2025-09-07 10:52:31
naviTitle:     PHP processes
title:         What are PHP processes?
lead:          'This is what you need to know about PHP processes and our implementation.'
group:         platform
stack:         all
hideExamples:  yes

---

## About PHP processes

Unlike frontend JavaScript, which is running in the web browser, PHP scripts are executed on the web server, and the output is then usually delivered to the web page as HTML.

A PHP FPM process is a reserved computing resource - think CPU. Each PHP process can respond to one concurrent PHP request at a time. Unlike with php-cgi the PHP FPM process stays alive.

## Our implementation

fortrabbit utilizes FPM (FastCGI Process Manager). Besides the allocated PHP memory we spawn a number of persistent FPM PHP processes with each scaling (hosting plan).

- See [Specs Uni Apps](https://www.fortrabbit.com/specs) for actual numbers
- See [Specs Pro Apps](https://www.fortrabbit.com/specs-pro) for actual numbers

## PHP processes, PHP requests and PHP response time

- By design PHP requests should only take a very short time to execute.
  - If it takes longer, it’s likely your code is slow. Refactor it for performance!
- Aim for 250 ms or less on average.
  - Our Dashboard shows you PHP response time metrics.
- A new incoming PHP request will have to wait if all PHP processes are busy.

### A simple example

- Your App has 2 PHP processes
- 1 PHP request takes 500 ms to execute (PHP response time)
- So each PHP process can handle 2 PHP requests per second
- Since there are 2 PHP processes, the App can deliver 4 PHP requests in 1 second

### A bad example

- Your App has 4 PHP processes
- 1 PHP request commonly takes 1000 ms (1 second) - which is too long
- 1 page view of your website creates 3 PHP requests
- 3 website visitors at the exact same time can bring the website down

## PHP processes VS page views

Page views refer to a single request made by a user to view a page on a website. This request will likely involve a number of HTTP requests, such as images, HTML, CSS, and JavaScript files, all of which must be downloaded from the Apache web server. 

A page view will likely also require the server to execute any PHP scripts associated with the page, which translates into one PHP process for each PHP script.

For many websites the ratio between page view and PHP request is one to one. But often one page view will generate multiple PHP requests, for instance by making different AJAX calls to the backend to query the database, returning API requests, or transforming images.

## PHP processes VS max execution time

Sometimes clients ask us in support to increase the `max_execution_time`. While this might help to execute long-running tasks, this also blocks PHP processes for a longer time. See the examples above.

That's why we usually recommend lowering the time it takes to execute PHP to be able to serve your website to your visitors simultaneously.

Our Pro Stack has [Workers](/workers) to offload long-running background tasks.
