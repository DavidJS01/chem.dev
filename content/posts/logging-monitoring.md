---
title: "I Finally Know Why Logging & Monitoring Matters"
date: 2023-08-03T14:49:09-04:00
draft: false
---

## Background

I am a relatively new engineer. At the time of writing this (August 3rd, 2023), I have 13 months of experience as a data engineering intern, and 6 months of experience as a junior data engineer. 

All of my work experience has been in the more software-engineery flavor of data engineering, while my current job is more like "Junior software engineer, data" in reality.

For my two internships, logging and monitoring was already in place. Sure, maybe logging wasn't executed the best with a lot of noise pollution in our logging platform, but at least the data was available on that platform to review.

I heard other engineers talk about how implementing logging and monitoring was high-value if done right, but I didn't quite understand why that was the case intuitively.

Until at my first junior data engineering job.

## Invisible AND Silent Fires, Everywhere

The heart of my company being able to operate and achieve its goal is through data. But, our core systems that process data is held together by [purple Elmer's glue](https://www.target.com/p/elmer-39-s-2pk-washable-school-glue-sticks-disappearing-purple/-/A-17088980?ref=tgt_adv_xsp&AFID=google&fndsrc=tgtao&DFA=71700000012510733&CPNG=PLA_Seasonal_Priority%2BShopping%7CSeasonal_Ecomm_Home&adgroup=Seasonal_Priority+TCINs&LID=700000001170770pgs&LNM=PRODUCT_GROUP&network=g&device=c&location=1021377&targetid=pla-306616797291&ds_rl=1246978&ds_rl=1247068&gclid=EAIaIQobChMIm4L0gJbBgAMVHDfUAR29KgQiEAQYASABEgJk3vD_BwE&gclsrc=aw.ds) and [pipe cleaners](https://www.amazon.com/Craft-Pipe-Cleaners/b?ie=UTF8&node=8090830011). 

Many, many bad trade offs were made when the original team designed our core systems - one of them being very minimal monitoring and logging.

### Peering Inside an Opaque Box

#### Current Problem
Currently, absolutely *no* logging is set up in our critical project that impacts the **entire** system.

Any bad data that slips through the cracks will seep all the way through every system component, eventually resulting in bad information presented to our users, creating a lack of trust in our core application.

#### Current Solution
Every day, an engineer in the support/infrastructure team logs on to >10 virtual machines and manually checks for errors. This includes viewing 4+ logs for each virtual machine, resulting in, at minimum, 40+ logs to manually review each day.

This raises many big problems:

- Human error can result in critical errors not being reported
- This is not scalable as a company that wants to significantly grow and expand
- This takes away a significant amount of time from that engineer's work day 
- Response time to critical production bugs is slowed down by this manual process

### Result of No Logging or Monitoring
I have seen:
- data issues not addressed for a long time, because no one knew there was an issue 
- bugs not addressed for a non-insignificant amount of time because no one knew about those bugs
- that engineer take PTO for a month and that pushed the burden of something that should be automated onto others
- distrust in the systems that power our application

### Moving Forward
I am currently working on a ticket for adding monitoring and logging! 🎉

This will reduce the burden on many departments, so I am excited to implement this. 

Considering the stress that the lack of logging has caused on both myself personally and my team, I am beyond ecstatic to remove that opaque box with something a little bit more see-through. 

Not being able to monitor our critical systems as a result of no logging has caused me more pain that I thought, and I will never take logging for granted ever again.

Hopefully by the time I'm done with the ticket, I will have transformed our system from being held together with purple Elmer's glue and pipe cleaner in an opaque box to being held in a somewhat transparent box :)
