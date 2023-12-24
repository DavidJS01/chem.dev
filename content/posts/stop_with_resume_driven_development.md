---
title: "Please, Stop With Resume Driven Development!"
date: 2023-12-23T17:03:43-07:00
draft: false
---

I'm begging you. Please stop with resume driven development.
![Homer from Simpsons Begging](/please_stop.gif)

# What's The Problem?
You're a small startup that creates a web-based SAAS product for clients. Your application read/writes to a DB.

You ingest that data, apply transformations to it, and output actionable new data objects on the web app.

You don't have many clients, maybe 10 or 15. Traffic is relatively light.

Do you need to overengineer a solution that will scale to thousands of clients, handle petabytes of data transfer
from the application to the DB in a single day?

**No. You don't.**

Do you REALLY need a ton of microservices, kubernetes on all the things, etc? No! 

## If You Don't Need It, Why Do You Do It?
### Resume Driven Development!

You're an HR recruiter. Your job is in recruiting, not tech. Which sounds better?

> "I created a microservice that handled thousands of user requests a day using Go, Kubernetes, GraphQL and Openshift"

> "I created an API that handled thousands of user requests using Go, Docker, and AWS EC2."

### "Cooler" Work!

I read an internal document that, quite literally, stated "Kubernetes: Cool Docker". For people not aware,
Kubernetes is NOT cool Docker. They have different purposes from one another.

The allure of working on more "cutting edge" tech is understandable; it sounds cool, and all the cool companies
use that tech too. I get it. It sounds cool, and you likely will learn a lot.


**"Cooler" work/tools is not always appropriate.**
> Most companies do not deliver value in the same way as FAANG+ companies do. Use the appropriate tools at your scale to solve problems and innovate.

## Clarifications
I am not saying that if you are a small startup to not use those exiting, "cutting edge" tools.

I'm saying that if you don't have a legitimate need for those tools, then don't use them. ¯\\_(ツ)_/¯ 

Trying to shove those tools in a scale where they evidently are not needed and only slows time to delivery is so frustrating. 

It is so annoying to see this happen and watch many team members get slowed down on using these tools when they weren't needed in the first place.

## Final Thoughts
Please, just be reasonable and make pragmatic decisions.
