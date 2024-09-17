---
layout: post
title:  "Earthquakes: Learning by example"
thumbnail: "assets/img/learning_by_example.png"
tags: ["Machine learning","Earthquakes"]
---

### I.

Machine learning attempts to generalize from (labelled) examples. Typically, many examples. 
To beat the poor horse. Here is are "examples" in various ML applications.

| Input                   | Label               | Task                  |
|-------------------------|---------------------|-----------------------|
| picture of cat          | "cat"               | image classification  |
| "Je suis triste"        | "I am sad"          | translation           |
| Chess board state       | move, e.g. "H3"     | chess                 |

### II.

So, is machine learning useful for earthquake forecasting? 
...well, are there good labeled examples? 
In this case, we might have something like:

| Input                   | Label               | 
|-------------------------|---------------------|
| History of earthquakes  |   Number of earthquakes after a week |


In the observational record there are millions of earthquakes; tens of thousands earthquake sequences; thousands of large aftershock sequences; a handful of regional sequences; at best a single prior sequence and almost certainly no identical earthquake sequence. So what does this mean for the number of relevant examples? Ultimately, short of a miracle, there wont be an exact labelled example. The earth is annoying that way. Too bad.

### III.

It helps to think about image labelling. We might have a picture which we would like to label as "smiling dog trying to pick up a frisbee" (in case it wasn't clear, the picture depicts smiling dog trying to pick up a frisbee). Now interestingly, it does not matter whether the image is flipped horizontally. It does matter if the picture is flipped vertically, then it would be an "upside down smiling dog...". We can shrink or expand the space occupied by the dog. Hey, we could even swap the breed of the dog; it would still be a "smiling dog trying to pick up a frisbee". All I am doing here is listing some symetries that, for all intents and purposes, preserve the gist of desired label. This basic premise makes image labelling so successful! Of course I am brushing over clever design choices that leverage these symetries. Symetries in the data imply that many examples might share the same label. 

### IV. 

Back to earthquakes. Do earthquakes have symetries? Maybe?

Here is me rattling off some that I can think of:
Trading space for time. Potentially an aftershock sequence in one part of the world might evolve similarly to another one. Relatedly, translational and rotational symetry. Moving or rotating the reference frame. Scale independence-the number of aftershocks is independent of the size of the mainshock once the minimum magnitude of the catalog is removed. 

These symteries, I think, are key to this kind of reasearch actually working. With these symetries in hand, many more examples become relevant to generating a forecast (fancy speak for a "new label"). 

### V.

When I ask a senior seismologist to articulate a forecast, and the associated reasoning, I typically get a response along the lines of: 

"well there is no way to know because this is unprecendented, however, in general earthquake of this magnitude have 30 aftershocks above magnitude 4. Other smaller earthquakes in the area are quite productive. I expect the afteshocks to occur along the current lination formed by the early aftershocks." 

...typical vague seismologist answer. I know. But let me add some annotation.

"well there is no way to know because this is unprecendented [no exact label], however, in general [trading space for time] earthquake of this magnitude have 30 aftershocks above magnitude 4 [scale independence]. Other smaller earthquakes in the area [COMBO: scale independence and trading space for time] are quite productive. I expect the afteshocks to occur along the current lination [rotational symetry]." 

At least annecdotally, señor seismologist is using the same approach, build out and leverage symetries in the data.

In this sense, I do not think we are doomed as earthquake forecasters. Clever tricks that best leverage existing examples is the key to success in earthquake forecasting with machine learning. Now, I would take this a step further to say that knowing what these symetries are in the data requires a proper understanding of the physics, so unfortunately, we are going to have  to learn something along the way. 
Next time, I will discuss what happens when we actually have an example.