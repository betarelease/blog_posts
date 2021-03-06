---
layout: post
title: grok grack
tags: [ruby, rack, git, unix, github, chacon]
---

Recently I was trying to host git repository from an already existing
(non-bare) repository. I 
was looking for a solution that does not force me to create a bare
repository and does not 
require me to install apache or some such webserver on my machine.

I found a wonderful tool written by Scott Chacon called
[grack](https://github.com/schacon/grack.git). grack is a git server on
top of rack. Its elegance is in its design. It consists of few hundred
lines of a rack middleware (awesome!) and a 6–8 line config file that
allows you to host any repository over http. Setting it up on a local 
machine was really easy. Even hosting multiple repositories is trivial.

I discovered one quirk when my server was not accessible, was due to
binding it specifically to 127.0.0.1.
Avoid this and bind to the hostname instead.

Many thanks to Scott Chacon, Github, Rack and Ruby for keeping it so
simple.
