---
layout: post
title: 'Setup your own intruder detection system .. in under 10 minutes'
tags:
  - security
  - face
  - machine learning
  - AI
  - DIY
  - python
---

I was looking to build a security system that captures pictures and alerts me when an intruder walks in to my home or is ringing the door bell. My constraints being:
* Off the shelf wireless IP camera
* Local Ubuntu box to process those images
* Ease of maintenance and management
* Secure, to the T.. for Security

I found a few options for wireless IP cameras on Amazon and that setup was easy. I was looking to do more - since these cameras send you notifications on each detected motion. I wanted to be alerted and told if this alert is worth looking at - meaning if there was infact an intruder attempting to break in.

I looked for tools and found a lot of material. Mainly though,
all thanks to [Adam Geitgey](https://medium.com/@ageitgey) I found [Machine Learning is fun](https://medium.com/@ageitgey/machine-learning-is-fun-80ea3ec3c471) . I was able to learn multiple approaches to implementing face recognition solutions from his blog and from all the links he posted.

In summary of his multipart post, I had choice** between
 [OpenFace](https://github.com/cmusatyalab/openface) or the [face_recognition](https://github.com/ageitgey/face_recognition#face-recognition).

 Adam wrote face_recognition as a python application which combines the aspects of face detection, analysis and recognition for easy installation and use. Face recognition claimed that it was easy to setup, ran on the local ubuntu box and was hence secure since it did not be exposed to the internet.

I chose to use face_recognition, and it was really simple to install and learn. I ran into only a couple of issues but was able to resolve them easily. This post details how I made it even simpler.

I started by following instructions on the [github README](https://github.com/ageitgey/face_recognition#face-recognition) but did not want to setup python3 on the system - and was looking to use pyenv instead.

Mainly the issues were around installing '[dlib](https://pypi.org/project/dlib/)' - "Dlib is a modern C++ toolkit containing machine learning algorithms and tools for creating complex software in C++ to solve real world problems".

I found instructions to setup dlib on [this gist](https://gist.github.com/ageitgey/629d75c1baac34dfa5ca2a1928a7aeaf) -
but was not thrilled since I did not want to use python3.

Further search yielded a clue -  [instructions to install with pyenv on mac]( https://medium.com/@kojiitp/install-dlib-with-pyenv-d348cb402ac5).

I converted those instructions to make them work on Ubuntu as follows:

```python
pyenv install 3.6.3
pyenv versions  #ensure it shows versions you have including the recently installed 3.6.3
pip install dlib
pip install face_recognition
```

I documented it all in  [Dockerfile](https://github.com/betarelease/face_recognition_docker/blob/master/Dockerfile) as a reference. I realized that I needed to install cmake before dlib could compile. I will keep on updating to make this more automated.

And that's all. I was able to run face_recognition via command line.

I intend to try out the [VM for face_recognition](https://medium.com/@ageitgey/try-deep-learning-in-python-now-with-a-fully-pre-configured-vm-1d97d4c3e9b). It is a little heavier install and will require more resources but allow me to do more machine learning.

Face Detection seemed so complicated in the beginning but with all these tools it has become so easy to use - I am amazed!!


** _There are options that allow you to use the cloud - [AWS Rekognition, Google and many others](https://www.kairos.com/blog/face-recognition-kairos-vs-microsoft-vs-google-vs-amazon-vs-opencv) - this post is limited to setting it up on a dedicated box. Setting it up privately also protects the privacy of the owner - after all this is about security_.

P.S: Also it did not really take _me_ 10 minutes to do this from scratch....I read and read a lot for more than a week before I could reach this point. In the end the setup turned out to be this simple. It served as a catchy title though :money_mouth_face:
