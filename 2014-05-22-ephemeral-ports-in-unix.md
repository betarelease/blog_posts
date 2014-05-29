---
layout: post
title:  Ephemeral ports in Unix 
tags: [kernel, linux, unix, ports, netcat, lsof, ephemeral, dynamic-range, rfc, ]
---

We learnt something while installing our app on a Linux box. We chose port numbers like 36601, 37601, 38601 for a number of clones of this app. When we restarted the app we found that we were not able to start one of the clones on its assigned port(say 36601).

##Ephemeral Ports
Checking for all running apps did not help because none of the apps that we started were using that port. Our app continued to failed to start(giving us a error like "Address already in use"). We used 'lsof' to find what was going on and we found that this port was not in use. We used netcat(nc) to find that this port was being used for TCP:WAIT. We attempted to kill everything else that could possibly be doing TCP(thinking that a particular subprocess has held on to the port). But 'lsof' indicated otherwise. The process holding on to the port was not a zombie.

Digging further yielded a little nugget of information we had missed.

Port numbers starting from 32768 to 61000 are reserved for kernel processes.(Wish I had my OS book around at this time.) Yes clearly this was the problem. 

##Here is how it works for those curious
The port numbers 32768 - 61000 are reserved by the kernel to spawn ports for its needs. It starts consuming these ports while never reassigning a once used port till it reached the limit 61000 and then it rolls over and start from the beginning 32768. 

Although it is true that user-space processes need to use ports upwards of 1024 and user-space processes can use the above ports. With the caveat that the above behavior of the kernel might yield unreliable results. 

What was happening in our case was we started our app on the ports 36601, 37601, 38601 and were lucky because at that time the kernel was not using these ports. But in the time that we restarted our apps, the kernel needed assign a port or two for TCP:WAIT and since we had relinquished our port momentarily the kernel held on to it. Thus on restart our apps were not able to start up on an 'Address already in use'. 

##Lesson learnt here 

Use port numbers between 1024-32768 for all user space applications. Or else you will end with this unfortunate scenario. It is ok if this happens on your laptop but you certainly want to be careful about this behavior in production applications.

##Way more information about this here

- [Wikipedia - Ephemeral Ports](http://en.wikipedia.org/wiki/Ephemeral_port)
- [Linux - local port range](http://www.cyberciti.biz/tips/linux-increase-outgoing-network-sockets-range.html)
- [What is different about linux](http://ashok-linux-tips.blogspot.com/2010/11/port-numbers-in-linux.html)
- [Dynamic port range - RFC6056](http://tools.ietf.org/html/rfc6056)