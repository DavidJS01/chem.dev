---
title: "I Broke Production"
date: 2023-07-11T16:01:50-04:00
draft: false
tags: ["data"]
categories: ["Hard Lessons"]
---

# Oh no.
![Spongebob Production Meme](/programmerhumor-io-testing-memes-programming-memes-aa0ef8c9f5cafc6.webp)

I sat in my shower and cried for at least 30 minutes after the fact. But, how did I get to that point?

## The Situation

### Background 

The core of my company is being delivered data files every day from our clients. Most of my work lives in maintaining the application that ingests client data, transforms client data, and then pushes that client data to the client's own production database.

That client data is then used, after being transformed and cleaned, for a web application. That web application is both used by individuals at the client's organization and specific individuals at my company.

It can take the client a lot of time to onboard and use the application. The process of getting the client ready for data ingestion takes a long time, and it takes developer intervention to automate the loading of client files.

### My Ticket

During my sprint, I was tasked with loading one day's worth of data from the client. This ticket is the prelude to the daily automated file ingestion: internal stakeholders responsible for getting the client launched wanted to see one full days worth of data to quality check it.

### My Work

To complete that ticket, I struggled with getting my local developer environment to match the client's VM environment. Because of this, several runs of data ingestion triggered in the client's VM was pushed to prod (first issue) with bad data.

After resolving the issue and seeing that my data load ran successfully, I wanted to clean up that bad data before triumphantly showing stakeholders that I did the thing correctly!

The work of getting a client up and running particularly is the priority responsibility of another department and requires business context, but I was the only engineer on the data team that could support this effort.

I effectively was being split between 50% data engineering and 50% client implementation (second issue). I lacked business context and did not realize that the client was manually entering data into the application's front-end, so I assumed they were not using the application (third issue).

### My Catastrophic Mistake

Because the client only had one DB (only prod, fourth issue) and I thought the client was not live, I ran delete statements on the DB without using transactions..


![Spongebob You WHAT Meme](/spongebob-you-what.gif)

I know... now **that** is a new low for me.

## The Aftermath

### The Gathering Storm
Within maybe 30 minutes, a new group chat was created with people from at least 4 departments. My stomach started to sink: "Was the client using the application?"

I shared a screenshot of the delete queries I ran, and the reception was not positive (understandably so). 

### The Plan of Attack
I was in a call with several leaders across the business, including my manager, for ~3 hours trying to figure out a plan of attack on how to restore the lost data. The DB was able to be recovered to the point before I ran the delete queries, but ~20 records had to be recreated by tracing our API logs.

I felt so terrible, and someone on the call was sharing their screen, and they forgot that they were screen sharing when they typed to someone "This is a classic example of why we don't let engineers have access to production" :/ 

Despite the horrible time, everything did get restored to normal.

## Hard Lessons Learned
1. Pair a production database with a development database
2. When working on code that affects a very unfamiliar part of the business, **never make assumptions**. Ask a lot of questions and get clarification.
3. If you know part of the system is configured to only push to prod, consider cloning the production database and pointing to the testing cloned database instead.
4. **Ask for feedback on your approach** from technical leads before commiting, especially for work that could significantly impact the business.
