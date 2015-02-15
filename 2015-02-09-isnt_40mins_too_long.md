---
layout: post
title: Isn't 40 mins too long for a build?
tags: [mingle, build, ci, continuous delivery, cd,feedback]
---

Yes, our CI build on Mingle takes 40 mins.

Yes, Mingle is a ruby application. Yes, Mingle runs on Java using JRuby.

With all these superlative technologies Mingle CI build still takes 40 mins. And this is an improvement. And we are happy with it.

Now that I have made some bizarre statements let me explain what they mean(before you start calling me crazy).

## Some History

Mingle has been built in ruby and is deployed using JRuby/Java. Mingle was built originally to work with database technologies that did not support NoSQL. So, Mingle team went ahead and built their own way of constructing schemas on the fly. These schemas were further elastic - in that they could be changed on the fly - as your team becomes better or needs different measurements/metrics over time. This provided ultimate configurability to Mingle.

Given this technology and the fact that Mingle supports Oracle, alongwith it's favorite PostgreSQL, Mingle team became heavily reliant on testing - all through to the database layer.
Over the years we have been able to improve the granularity of tests as well as improve coverage. But such tight coupling makes it tricky to separate the tests from the database.

<div class="inset">
<b>Select is in fact broken on Oracle</b>
<br/>
<br/>
Mingle team quickly learnt that 'select is not broken on PostgreSQL' - but for Oracle it is an entirely different tale.
<br/>
Select with an IN clause when used with Oracle has a limit of 1000 results and we have had to apply that restriction on numerous of our Select statements(only for Oracle).
<br/>
Furthermore, Oracle does not do a very good job at optimizing SQL - thus making a few of our complicated JOINs unpleasant to the naked eye as well as irking some Oracle DBAs - and hence being too slow to be useful with large data. Given that Oracle feature, we depend on our tests to ensure that such behaviour is not broken.
</div>

## Completely test driven application

Mingle's design forces a number of our functional tests to hit the database. Some of our unit tests do this as well.

Along with the configurability Mingle also lets the user derive lot of mileage through reports and collaboration. Both of these are applications in themselves(and there are business around each of them). Since Mingle supports interactions at so many levels our acceptance tests become more involved and require journeys instead of one simple test here and there.

All this becomes even more complicated when the application changes hands through multiple generation of teams. Over time I learnt that some tests could use some major refactoring but the effort may not yet justify the improvement in performance.

Thanks to this suite of tests we have often found bugs/test failures in areas that seemed to be remotely connected to the features being built. Some of the deeper coupling became obvious to us.

Another thing that is unique about this build for Mingle is that we run all tests - ALL TESTS. - before we declare success. There is no regression test suite - which runs after the fact for 8 hours, there is delayed feedback cycle. 40 mins is all it takes for anything and EVERYTHING to pass. If you look at it this way, then WE ACTUALLY HAVE THE FASTEST BUILD POSSIBLE. We could and have delivered critical changes to Mingle in matter of a couple of hours.

## Highly Parallelized CI Build

If we are to run any portion of this build - which consists of 8500 units/functionals and about 3000 acceptance tests - in a single process - would be like [watching grass grow](http://www.watching-grass-grow.com/). We do not have that kind of patience. But we rely on these tests to point out complex relationships among features/modules. In order to derive the same value from these tests without waiting forever we decided to throw hardware at the problem.

We use [gocd](http://www.go.cd/) as our Continuous Delivery tool. gocd allows us to have a really configurable pipeline structure which we can configure to scale. What we have is a massively parallelized gocd build pipeline which runs these tests in under an hour. Even for such a long build we have invested about 100 cores of processing power hosted on really fast machines. We have started using [openstack](https://www.openstack.org/) to manage these environments thus saving us a lot of time in case of VM failures, and at times when we have to apply OS patches/upgrades.

With gocd we invested significant time in parallelizing tests. One of the features of the old generation test splitting strategy was a simple but homegrown parallelization strategy. We replaced this with a much more robust, but automated test splitting and balancing provided through [test load balancer - TLB](http://test-load-balancer.github.io/). Using this tool exposed our test dependencies. We spent significant time in resolving these dependencies and cleaning up tests to allow them to be TLB friendly - independent of order, side effects and environment agnostic. This parallelization brought the much needed cleanliness. Along with this we were able to add more agents to extract better build performance.

Now we run all tests in parallel - units, functionals, acceptance.

## Power consumption as a build metric

For the number of tests we run and the throughput we would want we could technically throw more hardware at this problem. But thta wouldn't achieve much. Especially throwing more hardware means buying more resources - racks, machines, networking etc. The constraints we face are not how much hardware we can buy but how much it costs to run more hardware. We are now limited by how much electricity we consume and how much network bandwidth we use at our data center - because that is the cost that rapidly grows. With newest hardware and its increased resilience - the comparitive costs justify adding more hardware, but for their power needs and space needs we cannot do so.

## Fast Feedback, Fast Feedback, Fast Feedback

But what about FAST feedback? you might ask. We also think the same - we want fast feedback.

Certainly 40 mins is long enough. How do we get faster then? How did we get faster in the past?

We previously had a 2+ hour long build.(I can feel the cringe). We took it upon ourselves to shorten it slowly but surely.
A lot of effort went into profiling the build and identifying bottlenecks.

Another thing we did was reduce the number of stages in our pipeline and heavily parallelize. We also pruned/postponed tests that were for the slowly/not changing parts of the application. So frequently changing modules were built for fast feedback.

We picked a different tool to replace some of our acceptance tests and are migrating to using [capybara](https://rubygems.org/gems/capybara) and [webdriver](http://docs.seleniumhq.org/projects/webdriver/) for new tests or rewriting tests.

Slowly but certainly we are deprecating/deleting acceptance tests that are flaky/broken or in general do not seem focused enough. Along with these tests that are not able to explain the features - succintly are moved over and rewritten in [rspec](http://rspec.info/).

This strategy has yielded great results for us. If we continue this exercise we can certainly make our build even faster.

I am hoping some of these insights will help you fix your build issues.
