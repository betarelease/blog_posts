---
layout: post
title: Pushing ElasticSearch logs to Papertrail
tags: [mingle, elasticsearch, logging, logs, log4j, papertrailapp, syslog, rsyslog]
---

Do you have your elasticsearch logs going to your logs folder? Are you still struggling to get them to syslog so that they could be
transported to a log-archive system like we did?

Here is our working solution(with rsyslog):

Add the following lines to your rsyslog.conf or equivalent level of log configuration file

```
$ModLoad imfile
$InputFileName /var/log/elasticsearch/elasticsearch.log
$InputFileTag elasticsearch
$InputFileStateFile stat-elasticsearch
$InputFileSeverity error
$InputFileFacility local3
$InputRunFileMonitor
*.*   @<%= papertrail['url'] %>:<%= papertrail['port'] %>
```

We use the above recipe to tranport our logs from our elasticsearch server to papertrail log application. We tried configuring elasticsearch logging with log4j syslog appender but without success. Many of the links and search from elasticsearch documents indicated that it should work - but to our dismay we found out that it does not.

We tried going through the route of using remote-syslog2 from papertrail to push specific log files to syslog - but it required us to install ruby on that machine.

The above recipe relies exclusively on rsyslog configuration and works like a charm. If you have multiple log files in the log folder using wildcard characters like the following also works

```
$InputFileName /var/log/elasticsearch/*.log
```

We were able to push even the slow search index log along with the plain old search log.

Hope this helps someone who is struggling like us.
