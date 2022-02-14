---
layout: post
title: Investigating AI datasets - OpenImages
---
<img src="/images/2022/Banana.jpg" class="fit image">

##### (Disclaimer: This post is also featured on the [Data Culture](https://dataculture.dk/stories) blog)

Today's statistical algorithms, including a majority of machine learning and AI techniques, rely on large amounts of training data.
Understanding what is in these datasets, how they are collected, and how different groups are represented is important, as biased data often leads to biased algorithms.
Inspired by the recent work of [Emily Bender et al.](https://dl.acm.org/doi/pdf/10.1145/3442188.3445922) and Kate Crawford's new book [_Atlas of AI_](https://yalebooks.yale.edu/book/9780300209570/atlas-ai) I decided to look at look at one popular dataset - the [Open Images Dataset](https://storage.googleapis.com/openimages/web/index.html).
Open Images is a dataset similar to ImageNet, it contains approx. 9 million images, annotated with image-level labels, bounding boxes, segmentation masks, etc.

(Research)[https://arxiv.org/abs/1711.08536] has already shown that Open Images has issues when it comes to where images are from, with more than 50% of images coming from just 4 countries (US, UK, France and Spain).
However, here I want to dig a bit deeper into the dataset itself.
I'm not interested in where images are from, rather I want to know what they actually depict.
For instance, the image above is a random sample of the banana class. 
There are images of single bananas, stalks of bananas, banana bread, somebody using a banana as a phone, and even drawings/art of bananas. 
In total there are 743 images of bananas in the dataset.

<img src="/images/2022/top10-labels.png" class="fit image">

Banana is just one type of label in the 601-label large dataset.
If we look at the 10 most popular labels (see figure above) we see that around 800.000 images has the label "Person" (~ 9% of all images), followed by clothing with 600.000 images (~ 6.6% of images).
We also see there are also more images labeled "Man" than "Woman".
Here, we also notice a peculiarity, for instance, there is a label called "Human face". 
I'm not sure why "Human" is added, because there are no other types of faces in the dataset.
Anyways, that got me thinking, what other "Human"-like attributes are in the dataset?

<img src="/images/2022/human-like-labels.png" class="fit image">

In the figure above we see there are 13 different "Human x" type labels, from Human face, to hair, body, nose, eye, beard, to foot.
By far, the most popular, and larger than all the other combined, is "Human face".
The least popular is human foot, with only ~ 1000 images (but still more popular than the "Banana" label). 
The focus on human faces rather than any other body part tells us something about which purpose this dataset was assembled for, and most likely it looks like its for facial recognition purposes.
Please note that developments in facial recognition algorithms have been heavily funded by various defense and intelligence agencies. 

If we dig further into the "Human x" categorie and plot a random subset of the images labeled with "Human beard" we see that it mainly represents one specific demographic - mainly the beard of white men.
Its no wonder that 

<center>
<img src="/images/2022/Human_beard.png" class="fit image">
<img src="/images/2022/Man.png" class="fit image">
</center>

But that is not all. 
There are other issues....(TBD) 

### How did we do it? (Technical details)

Code is on [github](https://github.com/vedransekara/satellite-imagery-voting-records).
