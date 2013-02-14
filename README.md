# About

RapGenius wrote this [blog post](http://rapgenius.com/James-somers-herokus-ugly-secret-lyrics) 
about how Heroku's routing mesh only allows one concurrent request / dyno. Regardless if the dyno can 
handle more than 1 concurrent request at a time. 

Edit: After reading the blog post more carefully, it really only affects one process/request based
architectures like Ruby on Rails, PHP, Django, etc. Evented architectures are singled threaded
so they don't suffer from the same concurrency problems.

[Source code on GitHub](https://github.com/mostlygeek/heroku-concurrency-test)

## About The Test

The button below will fire 15 concurrent HTTP requests to the server. The server (node.js) will 
wait 2000ms before returning an "OK". 

Depending on the browser you are using you will see all requests appear in an `in-flight` status.
Then they will come back in batches of 5 or 6. This is normal behaviour as modern browsers will
fetch things concurrently. 

To get around caching, a little cache buster is appended to the URLs.

The round trip times are rounded to the nearest 200ms to make it easier to see groups 
of concurrent request. 

## Interpreting the Results

If results come back sequentially at times of 2000ms, 4000ms, 6000ms, etc. then 
the routing mesh only allows 1/req per dyno. 

If you see groups of requests come back at the same time then each dyno can 
handle multiple concurrent requests.    
