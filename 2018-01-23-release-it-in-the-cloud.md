---
layout: post
title: Release It - How it changes with Cloud tech
tags: [cloud, production, release, reliability, scaling, failure, disaster recovery]
draft: true
---

### Multithreading is complex :
"Moving away from 'fork, run, die', model brings higher capacity but only by introducing a new risk to stability" - compare how this is not the case when using JVM and multithreading is only one way of achieving better utilization. With any interpreted language and current cloud tech, multithreading is no longer the necessary evil. You can choose to go with multithreading technologies or use technology that scales differently. You can stay away from complexities of multithreading.


### Be wary of third party code:
Yes trust and test, But keep your code as independent as you can. So that you can indeed plug and play.



### Unbalanced Capacities:

Does not make sense as written in this book - due to autoscaling and DR options on the Cloud

Purge stale data like logs, cache, other activity:
Use papertrail, logDNA, splunk instead. You will never need to roll logs after and the default logging mechanism will be sufficient.

### Wasted Space in HTML
Techniques available to do this:
* gzip - https://stackoverflow.com/questions/11641923/transfer-encoding-gzip-vs-content-encoding-gzip
* webpack
* minimize

* https://en.wikipedia.org/wiki/HTTP_compression - ref : Problems preventing HTTP compression

* https://medium.com/@BotmetricHQ/top-11-hard-won-lessons-learned-about-aws-auto-scaling-5bfe56da755f

* https://www.quora.com/How-long-does-it-take-to-spin-up-a-new-AWS-instance
* In our experience, best is under one minute, worst (assuming on-demand and not spot) is around six minutes, and average is around four minutes.  We launch mainly instance-store-backed instances.

* http://www.philchen.com/2009/04/21/how-long-does-it-take-to-launch-an-amazon-ec2-instance

* http://www.logicworks.com/blog/2017/12/common-mistakes-misconceptions-auto-scaling-aws/

* http://blog.flux7.com/blogs/aws/must-know-facts-about-aws-elb

* warm up your elb
* https://forums.aws.amazon.com/thread.jspa?threadID=76834
