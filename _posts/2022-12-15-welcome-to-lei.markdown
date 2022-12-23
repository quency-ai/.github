---
layout: post
title:  "Welcome to LowEndInsight!"
date:   2022-12-15 17:46:41 -0500
categories: lei update
---

## Everything Needs an API

This project has been in various states over the past 10 years or so - read more
[about](/about) the history.  But I'd always felt like a simple, publicly
available API would be part of the interface.

Recently found out about [RapidApi](https://rapidapi.com/hub) and as I'd been
considering pushing out the LowEndInsight library behind an basic API, figured
it was worth a look.

It is now up in "ALPHA" form, while we continue the buildout of the backend
services required to scale out in the event somebody actually wants to use
it. :heart_eyes:

You can find the start of this journey here:

[https://rapidapi.com/quency-ai-quency-ai-default/api/lowendinsight2/](https://rapidapi.com/quency-ai-quency-ai-default/api/lowendinsight2/)

We'll be working to update the documentation, release more marketeering on the
"why".  As Quency itself gets its footing we'll also be posting more about 
how were working to actually shift-left security, towards the developers who
are responsible for the code that ends up on our production environments.


## So What's in a Name?


Yeah, so the name...like all things has a bit of a story.  There is a lot of 
talk about "stacks", e.g., LAMP stack (meaning Linux, Apache, MySQL, and PHP -
sshh, I know I know).  But it got a few of us talking about the need to have
insight into the source code for the entire stack, and to be able to track
changes (expected or otherwise) as the software builds and is ultimately
delivered to downstream consumers.  Some one said that this leads to a "no end
in sight" situation - as not only is there more and more software in our
"stacks", but also the rate of change for each of those pieces is increasing.

I quickly picked up that as one of my favorite bands, ASG, has a song called
["Low End Insight"](https://www.youtube.com/watch?v=hIv10I2hMmY&list=RDhIv10I2hMmY&index=1).

And thus the idea that developers, project/product managers, need ways to 
identify risk in open source **before** it is added as a dependency.  The
source code being the "lowest" in the stack.  And the tool being the insight
needed to make informed decisions.


