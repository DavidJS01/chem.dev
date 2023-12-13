---
title: "Stop With the OOP Hivemind"
date: 2023-12-12T20:40:48-07:00
draft: false
---

**Preface:**
I understand that in a software team all that matters is that you all pick a convention, use it, and conform
to your team's standards.

What matters is gaining victories as team, not my own personal opinion :). In software, I truly believe
the team > the individual.

# Why Force It Everywhere?

## Situation
My team used to have a senior engineer that came from a very traditional, old business type of environment.

We all are products of our environment, culture at the moment, etc., so it is no suprise that he was a huge fan
of OOP.

And, to be fair, OOP done well can be great because it unifies developers under one terminology. Although, it isn't done well that often ;).

I had a situation where I had to create a very small project at work to read/write K8s configmaps. At the end of the project,
he complained that the small codebase was written in a "functional" (actually procedural) style.

## Solution
He said that it would be complex to maintain. His solution? Wrap every function under one single class. And keep everything else the same.

**WHAT?**

He literally put a wrapper around this procedural code, and then said that now it was better? At what point are you forcing OOP just to have it?

I can not imagine any benefit to including a class for this small, hardly used project.

It bothered me so much that OOP was forced upon this just to have it. Yes, we should conform to (unofficial) team standards. This still bothered me.

Maybe I just especially hate strongly opinionated thoughts/leadership without nuance. Such a dumb blocker in code review.

# Java Bleeding Into Go

Something much worse than actually creating a bunch of classes just to have them is to force OOP **terminology** into languages like Go.

It is evident the immense influence OOP had on both industry & academia because so many people on my old team packed Go code full of Java terminology.

Go code written by Java devs forcing Java practices onto Go is the ugliest, most hair-pulling Go code I have ever worked with.

That code is perhaps the biggest example of forcing something just to have it. I don't know why, but that bothers me.

The worst experience was putting a PR into a modular, simple project and then having a more experienced dev block my Golang PR until I implemented Java
conventions, names, many refactors beyond the scope of my ticket to fit his Java design, etc.

Absolutely infuriating.

I partly blame the book Clean Code.

# Be Flexible, Pragmatic, and Reasonable

I am **NOT** demanding all codebases I work on to be procedural, functional, etc. Although, I would love more functional work :)

I am politely asking to be pragmatic on how we approach problems and how we approach designing codebases. Take the approach that makes
the most sense for the team.

Ultimately what matters at the end of the day is that you solve problems and/or create value for stakeholders/customers.

But, maybe consider reflecting on your opinionated thoughts with the context of your specific situation. Strongly held thoughts
without nuance can be dangerous, or just annoying, to deal with :).

And PLEASE stop shoving the square OOP shape into every triangle hole you find.
