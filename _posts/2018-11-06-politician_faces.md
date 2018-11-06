---
layout: post
title: The generic face of danish politics
---
<img src="/images/2018/faces_front.jpeg" class="fit image">
As Denmark is getting closer to the next elections the debate about refugees, migrants and their descendants has yet again resurfaced and is beginning to turn sour. 
Disillusioned with this development and trying to get my mind off the issue, i wondered what the average politician looked like. I was thinking about something along the lines of this [work](https://www.businessinsider.com/faces-of-tomorrow-2011-2#at-sydney-university-2) or something like this piece by [Soumitra Agarwal](https://github.com/SoumitraAgarwal/Formula-1).
After a quick online search, where i was unable to find much work on faces of politicians, i decided to create my own.
The basic idea is to take lot of portrait pictures, overlay them, and take their median. 

It's actually a quite simple process and the code is freely available on [Soumitra's github](https://github.com/SoumitraAgarwal/Formula-1).

To start with the pictures; I scraped the faces of Danish politicians from the web page of the [danish parliament](https://www.ft.dk/da/medlemmer). 
This gave me the following mosaic (below mosaic shows an example of some of the scraped images).
The parliament has 179 members, composed of members from multiple political parties.

<center><img src="/images/2018/faces_raw.jpeg" class="fit image"></center>

The fact that all images were taken from the same webpage meant that they all had the same dimensions - making my life easier.
Normalizing the faces, however, is still required. 
This basically means that faces are centered and adjusted (using a pre-trained model) to fit within a specific region.
Effectively, this standardizes faces and makes them comparable. 
Below image shows the normalized versions of faces.

<center><img src="/images/2018/faces_adjusted.jpeg" class="fit image"></center>

After standardization we can overlay the images, calculate the median, and get the face of the average danish politician.
Here we see that the average politician is white, has stereotypically masculine features, and wears glasses.

Dividing pictures into male and female politicians (at present no danish politicians identify as non-binary) we see that the average male politician again wears glasses, is clean-shaven and wears a dark suit. 
The average female politician has blue eyes, does not wear a suit, and is portrayed in front of a warmer background.

<center><img src="/images/2018/faces_median.jpeg" class="fit image" style="width:500px"></center>

We can dig further down and separate portraits with respect to political affiliation to get the average face of each political party.
Overall the portraits are very white - I'm actually shocked by the lack of diversity (this might actually explain some of the toxic discourse in parliament).
It would be interesting to compare these images to the face of the average Dane - if you have an open collection of images or know where to find one of Danes please share them.
I'm planning to expand this work and start looking into the average face of danish actors.

<center><img src="/images/2018/faces_parties.jpeg" class="fit image"></center>