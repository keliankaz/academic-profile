---
layout: post
title:  "Analog twins"
thumbnail: "assets/img/learning_by_example.png"
tags: ["Aftershocks","Earthquakes"]
---

There is this rising trend in earthquake research for so called digital twins. Simulate as best we can a large earthquake that has just occured by throwing and ungodly amount of compute at it. See more about this here: https://www.nature.com/articles/s43017-023-00469-y. I'd like to one up this proposition, lets study analog twins...

Analog twins?

Earthquake repeaters are common: small earthquakes that occur regularly and have identical waveforms. These are cool, and have been studied to death and more often than not they are so small that it is difficult to say much more about their source physics. I would make the case that larger repeat events might tell us quite a bit about process. These bigger quasi-repeat events, I will call analog twins.
Oddly enough, I could not find any real attempts at screening the existing record for bigger repeaters (see  This and This?). So I went ahaed and did just that. 

### I

Before I talk about what I found, let me first interject and highlight why we do not expect large earthquakes to repeat. Earthquakes are like drunken men stumbling out of a bar--despite starting at the same place, the odds of ending up in the same place are small, and most won't make it far. This type of reasoning underscore pessimism about earthquake prediction. We really don't know where an earthquake is going to stop; small nudges along the way can have a huge impact on the final end point. Therefore, even if we could predict where and earthquake occurs, we really have no way of knowing how big it will become. Now if two of our drunkards, hem hem, earthquakes end up in the same place we should raise an eyebrow. Is this just a stroke of shear luck? If they stumble right out of the door and did not get so far, maybe. But what if they got accross town?

### II

Back to earthquakes. I dug up the SCARDEC database. It's a database of large earthquakes for which the source time functions (roughly speaking how much the earthquake was slipping at any given time throughout the rupture), screening for earthquakes with the same size in the same location, same orientation and slip direction. I removed earthquakes that were close in time. I get in the ball park of 30 pairs of earthquakes. Of these a handful even have the same source time function. Cool! 

Here's the code...

``` python
DT_min = 10 * 365
dlat = 0.2
dlon = 0.2 
ddepth = 5
dM = 0.05
ddeg = 10

def difference(series):
    return np.abs(np.atleast_2d(series.values).T - np.atleast_2d(series.values.T))

repeated_bool = (
    (difference(scardec.catalog.origin_time)/np.timedelta64(1,'D') > DT_min) &
    (difference(scardec.catalog.longitude) < dlon) &
    (difference(scardec.catalog.latitude) < dlat) &
    (difference(scardec.catalog.depth) < ddepth) &
    (difference(scardec.catalog.mw) < dM) &
    (difference(scardec.catalog.strike1) < ddeg) &
    (difference(scardec.catalog.dip1) < ddeg) 
)
```

Remember these earthquakes that are big, generally ~10km patches failing during the observational record. 
What are the chances?! Let's actually grapple with this question. First off these so-called analog twins are quite rare, we started off with a few thousand earthquakes. Is it plausible that these events just happened to occur in the same place by chance? At some level this is a really tricky question to answer. A we can loose answer to this question by just multiplying the total number of events in the database by the product of propotions of each of the above subsets outlined above (what a mouthfull). So ~4k events multiplied by the proportion that are not close in time multiplied by the proportion in the correct latitude range, multiplied by the proportion in the right latitude range, multiplied... and so on. Doing this I get the chances of even one repeat to be 1e-6. This seems low to me. This estimate assumes all these factors are IID which is obviously wrong, e.g. earthquakes at a given longitude is more likely to be at specific latitude. That said, even considering the joint probability of earthquakes in the selected time range, latitude, and magnitude leaves you with sub-1 expected events. Are you convinced? I think I am.
I made sure that these earthquakes are more than 10 years appart, so no I don't think these can be aftershocks of eachother. M6ish earthquakes, with ~1m of slip every 10+ years is in the right ball park for given typical slip rates on the order of cm's/year. 

### III

If some hand of god, or call it physics if you care about splitting hairs, led the earthquakes to have the same rupture, what happens after? Are the aftershocks also similar? I'll be honest, it would be really good news for me if they were. I've been off at various seminars that we can use the characteristics and/or the location the mainshock to better forecast the aftershocks. You would hope that when the situtation, in terms of seismologically observable attributes, is almost identical that the aftershocks also be quite similar. From looking at the data this is true, but only to a limited extent. There are still up to factor of 15 variations in the number of aftershock, though many are quite close. 
eeeeek.  A real peice of humble pie for us forecasting folk. 



