---
layout: post
title:  EbDeployer
tags: [eb_deployer, EbDeployer, eb_deploy, elastic beanstalk, amazon, aws, cloud, mingle, services, saas, featured]
---

If you have been following this blog, you may have been curious what I have been upto recently. Few weeks ago we(Pengchao Wang and I ) got selected to speak at the aws re:invent 2013 conference to speak about our tool eb_deployer.

Here is a little bit of insight on what it is about and why we wrote it.

<!--more-->
## So you want to use the cloud
So like everyone else wanted to put our software on the cloud and stop maintaining a shrinkwrap version. Among other things we want to develop many features - fast and have them enhanced as we go. We started building or transforming Mingle to be a SaaS but slowly. When we wanted to add new services that did not belong in Mingle we build them as separate services. This is how Cycle Time Analytics and Single Sign On is built for Mingle. While putting these services on the 'cloud' we started mending our ways to follow the amazon way. The first attempt along that way was to use CloudFormation and we chose that because it gave us ultimate flexibility and control.

## But do not want to do any work
But CloudFormation meant a bit of work to maintain the definitions and be meticulous about turning each and every knob and getting it right. We did not want to do any work for the new services we were building since we did not have the constraints of Mingle.

## You should think of Elastic Beanstalk
On closer look we found Elastic Beanstalk - a technology meant especially for our needs - minimum work maximum benefit. We started building and deploying to Elastic Beanstalk stacks(as they are called) and were amazed with how much we could get accomplished with little effort. Elastic Beanstalk gave us basic technology stack along with autoscaling, basic monitoring and logging all in the bundle. But we wanted to do more with it - think continuous delivery.

<!--more-->
## And use EbDeployer
Elastic Beanstalk allowed us to worry less about deployments more about our code. When we started deploying regularly we found Elastic Beanstalk wanting in some areas. So Pengchao Wang started building now what we call EbDeployer - a tool for automated blue-green deployments for your continuous delivery cycle.

EbDeployer as it is built relieves you from the pain of writing any Elastic Beanstalk api code. All you need to do is package your application, define some basic parameters and deploy(or eb_deploy). You are done!

You can find out more from our talk at [aws re:invent 2013](http://blogs.aws.amazon.com/application-management/post/TxZ8FZHFP6EYOQ/AWS-re-Invent-2013-Sessions-Now-Available)

<iframe src="http://www.slideshare.net/slideshow/embed_code/28428616" width="600" height="400" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px 1px 0; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/AmazonWebServices/aws-elastic-beanstalk-under-the-hood-dmg301-aws-reinvent-2013-28428616" title="AWS Elastic Beanstalk under the Hood (DMG301) | AWS re:Invent 2013" target="_blank">AWS Elastic Beanstalk under the Hood (DMG301) | AWS re:Invent 2013</a> </strong> from <strong><a href="http://www.slideshare.net/AmazonWebServices" target="_blank">Amazon Web Services</a></strong> </div>

Also checkout the tutorial at [EbDeployer](http://ThoughtWorksStudios.github.io/eb_deployer).
