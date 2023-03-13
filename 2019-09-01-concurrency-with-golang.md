---
layout: post
title: 'Concurrency with Golang'
tags:
  - go
  - golang
  - programming
  - concurrency
  - sharing
  - channels
  - pipelines
  - PKS
  - Pivotal
  - Tanzu
  - kubernetes
  - sticky
image: images/golang_concurrency/golang_concurrency_talk.png
---

At Pivotal when we were building [Pivotal Kubernetes Service(PKS)](https://medium.com/rafay-systems/getting-started-with-pivotal-kubernetes-service-pks-985119bd6697) the most critical path of the process was the Release Engineering team. This team was tasked with making sure all the components integrate well together and we always produce a working version of PKS.
This PKS was rebranded to become the Tanzu offering from VMware which has greatly evolved since then.

Doing this had many challenges we needed to overcome. Among them was one specific challenge of making our process of building all the components efficiently and with great speed. This process took close to the entire working day once started(7-8hours approximately) and would often produce an error result. An error result meant we had to run the whole process again before we start the testing on variety of clouds.

In this talk, I explain the challenge in what we were trying to do and how we overcame the hurdle of long running build cycles. This was presented at the GoNYC meetup.

## Talk at GONYC

[![Golang Concurrency and Asynchronous Processing](/images/golang_concurrency/golang_concurrency_talk.png)](https://www.youtube.com/watch?v=z47m0okLtWM "Golang Concurrency and Asynchronous Processing")
My talk was the second talk at the meetup. Please scroll past minute 31:04 to listen to my talk.

## Slides for the talk

I attached the slides I used for this talk. Please provide feedback on how you liked the metaphor I used for this talk.
[Slides for the talk](https://docs.google.com/presentation/d/13JXnExSRZheBWpryJMvrHEixNqtb8z9V/edit?usp=sharing&ouid=105097944914173266944&rtpof=true&sd=true)
