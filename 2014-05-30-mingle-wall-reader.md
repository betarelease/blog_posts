---
layout: post
title:  Mingle Wall Reader - Sync your physical card wall with Mingle
tags: [mingle, rfid, arduino, mingle-api, mingle_party, httparty, http, hardware, hacking, featured]
---

Some time ago we built a hardware integration to Mingle. We did not talk much about it at that time. Hence this post to revive the talk about hardware interface with Mingle. Also about other hardware projects that were done in ThoughtWorks around 100days of hardware.

<!--more-->

## Physical Card Wall
Many teams work in the traditional way where they use a physical(real) card wall to manage their project. Having a physical card wall has its advantages:

- Tactile way of interacting with the cards
- Completely fluid and configurable card wall
(In fact you can have the card wall as big and as unique as you would like. More examples [best card walls](http://www.pinterest.com/thoughtworks/bestcardwall/))
- Evolutionary by nature
- Repurposable
- Uses paper and pen - User experience is very basic and refined

Although with all these advantages there is a need for digital walls and hence Mingle. What Mingle provides beyond the digital wall is a number of features that get you close to a physical wall while throwing in a number of freebies - reports, history, distributed agile management etc.


The teams that you a physical card wall with Mingle feel the need to keep it in sync with Mingle. Most of the times the people in the room with the physical card wall are relunctant or miss out on making the same changes on the web as on the wall. This has been a chronic headache for the project manager who now has to spend time managing these two sources of truth to get accurate reports. This also could be a big deterrent in adopting Mingle as well.

## Wall Reader
In order to bridge this gap and keeping these two entities in sync we recently built what we call wall reader(fondly the project is called 'Sugar').

Wall Reader interfaces with Mingle using RFID tags. Every card on the physical wall is attached(glued) and associated to an RFID card. These RFID tags can then be scanned using 'Sugar' which reads the tags and helps you move the cards in the appropriate lane on Mingle. 'Sugar' uses an arduino to interface with the RFID reader. This arduino is connected to a ruby engine which posts/receives data from Mingle via Mingle APIs.

We were able to build this very quickly using an off the shelf arduino kit from sparkfun.

## RFID readers and cards - Quirk
One gotcha while working with RFID readers and cards - the RFID reader we used operates at a certain frequency and the cards that come with it are compatible with it. The cards that are usually used for door access systems in offices use a different frequency range, need a more sophisticated reader. So your ID cards will not work as a replacement for the RFID cards with sparkfun RFID reader. Please check the specs before buying one of these. Our specs and SKU numbers are below so that you can buy the exact same things that worked for us.

- [Sparkfun inventors kit](https://www.sparkfun.com/products/12001)
- [RFID cards and RFID readers](http://www.instructables.com/id/Arduino-and-RFID-from-seeedstudio/)
(The RFID shields we used have been upgraded and no longer the same size or form. The above ones look like close replacement.)

##Demo

Here is a video demo of how it works.

<iframe src="//player.vimeo.com/video/97603955" width="600" height="400" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="http://vimeo.com/97603955">VID_20140607_105007_353.mp4</a> from <a href="http://vimeo.com/user4311546">Bill DePhillips</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

We have the code([wall_reader](https://github.com/ThoughtWorksStudios/wall_reader.git)) and design opensourced - so feel free to build one for yourself. If you have any questions feel free to reach out so that I can help.
