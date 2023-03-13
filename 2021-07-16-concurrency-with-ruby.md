---
layout: post
title: 'Modern Concurrency with Ruby'
tags:
  - ruby
  - golang
  - programming
  - concurrency
  - sharing
  - channels
  - pipelines
  - concurrent-ruby
image: images/ruby_concurrency/ruby_concurrency.png
---

When working on building a [log processing engine](https://github.com/jfrog/fluent-plugin-jfrog-siem) based on Fluentd we were facing serious stability issues. The daemon that processed the log used to run out of memory, it would crash or run the CPU to 100%. All this would mean a disruption in streaming the logs to the desired monitoring system. This in turn meant that the system health alerts and dashboards were losing current data. Ultimately this caused issues for customers in fixing problems that they observed. Overall a really bad scenario to put your customer in when all they are trying to do it observe the health of your product.

We went through cycles of fixing critical issues, adding stability fixes but the core problems remained and we were looking for a way to solve it for good.

Our analysis revealed that we were using an old version of ruby, an old version of the thread gem and we were not following the best practices recommended by the fluentd gem. If you would like to learn more about how we confronted the problem and how we solved it please listen to this talk that I presented in a few ruby meetups. This particular recording is from the Boulder Ruby meetup.
[How we made our Ruby app Concurrent Golang Style](https://www.youtube.com/watch?v=JAD2s2nMr_0)

## Slides for the talk

[Slides for the talk](https://docs.google.com/presentation/d/1hgLXleSytsdXCRo6R3fxqi9XFK7dAsO2/edit?usp=sharing&ouid=105097944914173266944&rtpof=true&sd=true)

This [siem plugin](https://github.com/jfrog/fluent-plugin-jfrog-siem_) is hosted on github and is open source.

If you are interested please download the code and make contributions.
