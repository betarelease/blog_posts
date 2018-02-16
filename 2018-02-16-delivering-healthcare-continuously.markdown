---
layout: post
title: '2 challenges, one healthcare app, continuously delivered (Part II)'
tags:
  - healthcare
  - glooko
  - fda
  - hipaa
  - regulatory
  - continuous Delivery
  - CD
---
(This post is the second and final part of a two-part blog and focuses on continuous delivery. [Previous post](http://betarelease.github.com/who-knew-healthcare-is-hard/) is )
Given that the healthcare environment has such strict regulatory requirements and legal boundaries it is a challenge in itself
to write good software that fits the bill. We also were looking to make this a seamless experience for our customers. Our platform was composed of a few parts:
* a mobile app that works on both iPhone/Android,
* web platform and
* an dedicated device that allows to integrate with out web platform for fast sync of data - in cases where a phone is not available.

Whenever we released a version we had to ensure that all these parts work together. Each release had to be traced as I mentioned in the previous post. But adding any such traceability threatened to reduce throughput and further demotivate the team
since not each collateral actually applies to every team member - for example developers who wrote server code were not interested in how the instruction manual was written and the customer support did not care so much as to how the developers tested their code(as long as they tested it). So we had to figure out ways to make this traceability a part of the process and make it seem less daunting, so that it would be adopted with open arms.

## Setting up and using CI for regulatory traceability

We started by identifying all the areas that need to have a traceable document/artifact in order to understand our process.
We found that more or less a bunch of those artifacts can be driven by how the work is planned and delivered. For start, we knew that developers on the web team were quite passionate about writing tests(as is natural in the ruby world), and it would be really easy to print a success/failure report and break the build based on these test runs. We started there.

We started by adding a CI server(goCD) which ran the builds and prepared the web platform for deployment. We also built it so that we could see the entire process as well as have the reports in a printable consumable form. Once the developers got familiar and convinced that they did not have to do anything extra we started building more into our CI pipeline.

The next stage was building a CI job to deploy the build on development and QA servers. Now since the development server was configured differently(had only one multi-app instance and used Capistrano) than the QA server(which was a clone of production and performed autoscaling), the first task was to automate it and remove any parts that needed manual intervention. We choose to use CloudFormation which helped us replicate the production environment and use the same strategy in both QA and production to perform infrastructure changes as well an deployments.

Once we got that setup working and error free we were able to hand it over to the testing team who could then drive what was being deployed on QA, when it was being deployed on QA, etc. - instead of being dependent on the development team to run these deployments. They started to have better control on their own systems and could plan their work better. At one point they got so good at pointing issues that they were able to even figure out when a particular build was an issue and when these issues started regressing.

Once the QA team could control the stability of builds on the QA server they were more than comfortable using data-driven automation to write more and more automated functional tests to replace their manual testing efforts. That is when QA team started to have somewhat of a predictable testing and regression cycle for each release. Having said that, since there were a number of human driven actions required to work with the phone apps there was some manual testing involved, but we now had the ability to postpone the activity to when it suited instead of depending on the manual testing.

Running every build through CI gave us traceability of releases, test reports and other artifacts that we needed out for a release.

## Setting up your Process Management system - or Fighting with the JIRA

In order to pass a regulatory audit you need to ensure that each product change is traceable. So we had to start from the beginning. We started by making our requirements documents version controlled. Once they were finalized we added them to JIRA as stories. We also had to ensure that anything that was added to JIRA never got removed/deleted - and luckily JIRA allows that. So we configured a flow for our stories to not have a delete option and the ability to be marked as abandon so that in case of an audit we were able to find the history about a feature and why it was never part of the product.

Having the flow configured in JIRA helped the team move faster. Additionally JIRA allowed integrations with github, zephyr(test management), code review tool (that comes built in with JIRA - called Fisheye). And all of these were then tied to confluence for reporting as well as managing releases.

This gave us a single platform to go look for a feature, how it was built, tested and released and then in case of an issue we could trace back our steps.

It may seem unreasonable(and it did to me) in the beginning, but having this flow setup and spending the time training paid off we started getting faster in dealing with the mechanics. Which in turn allowed us to focus on our product quality.

I also learnt that this fight with JIRA is by design - https://en.wikipedia.org/wiki/Jira_(software) - once I read that the name JIRA was coined after GodZilla ;)

## Release Frequently

Having these two tools setup well and after a few iterations of getting familiar with them, we started releasing frequently and more predictably. What that forced us to do was do the activities like:

* Writing Req docs at the beginning of the release
* Writing Release notes - at the end of the release
* Instruction manual - at the end of the release (based on need)
* Customer support collateral - at the end of the release
* Coordination with partners (and there are many) - during each release and during planning for the release
more frequently.
These seemed like hurdles but as we started doing them we were able to account for them in our release cycle and became chores.
In turn we got better at planning the release and if not well planned were able to pushback on release timelines/release contents.


In the process the team now has a better handle on how releases are done in a controlled environment without compromising the quality as well as the frequency of the releases.

Some handy references:
* [Martin Fowler's blog on ContinuousDelivery](https://martinfowler.com/bliki/ContinuousDelivery.html)
* [Principles of ContinuousDelivery](https://devopsnet.com/2011/08/04/continuous-delivery/)
* [Architecting Continuous Delivery](https://www.thoughtworks.com/insights/blog/architecting-continuous-delivery)
