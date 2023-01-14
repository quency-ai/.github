---
layout: post
title:  "Functional Contributors"
date:   2022-01-15 10:46:41 -0500
categories: lei
---

`All software builds on software.  Sounds obscure and abstract...well, because it is.`

## The World has a Problem

We are chasing the proverbial tail.  Our global cybersecurity posture is based on two huge fallacies - 1) defense is based on economically manipulated black-market attack vectors, propogated within and without nation-state actors, who are equally manipulated; and 2) the entire premise operates on "known" vulnerabilities - prevention and defense are hyper focused on the ability to scan for "known" holes and offense is the new luke-cold warfare.

[This is How they Tell Me the World Ends](https://thisishowtheytellmetheworldends.com/) is an incredible look into the brief history of how we got here - showing how it is near impossible to trace trace to knowledge, who is who in the zoo.

[Rohit Sethi at Security Compass and his essay on the need to focus on leading indicators](https://www.securitycompass.com/in-the-news/guest-essay-the-case-for-network-defenders-to-focus-on-leading-not-lagging-indicators/) highlight the reality that we're not really protecting the fort, nor preventing the holes, but are only looking for vulnerabilities as they become known by continually scanning against what has already been built.

[Mark Curphey sees a crash coming](https://blog.crashoverride.com/a-security-tools-crash-is-coming) in the secuity tools space. While consolidation is a natural contraction in technology's rapid evolutionary environments Mark points out that "budget freezes" induced by global economic changes will affect how organizations spend.  And as the reality of this awkward security posture becomes clearer - many of those companies will be exposed, as simply not effective.  More to the point, [Mark also highlights the obvious](https://blog.crashoverride.com/why-supply-chain-security-is-so-much-more-than-open-source-code-and-cves) that even with SBOM/BOM technology in place and clean bills of health, organizational dependency on "services" (e.g., IaaS, PaaS, and SaaS level software) plus the lagging indicator problem of "known" vulnerabilities creates a massive false sense of security.

## Software Engineering has a Problem
All software builds on software.  Sounds obscure and abstract...well, because it is.  I'll go ahead and take the position that Open Source software (OSS) is generally and ubiquitously foundational - at the OS, application, language, library, framework levels.

While there are supporting organizations and foundations (e.g., Apache, Linux, and Cloud-Native Compute, etc), that create ecosystems for commercialization, the closer you get to development and software engineering - the greater the dependencies become of open source.  There are very few software engineers out there developing solutions from scratch.  And even if they were, they're likely doing it with open source language and tool depenendencies.

[I am not a supplier](https://www.softwaremaxims.com/blog/not-a-supplier), a post by [Thomas Depierre](https://github.com/DianaOlympos), is a massive right-hook to our understanding of the complexity that exists between open-source software contributors and maintainers and downstream consumers and users.

Our software-supply chain structure is strained, and new demands for formality in the relationship between producers and consumers is exacerbating the load on both ends.   Nevermind the strange assumptions that SBOMs are a direct security construct, but the loose contract between consumers of OSS leads to more assumptions about the supply-chain.  Specifically, the notion that an upstream project will act as a maintenance function in a time of need (bug, feature or security issue).  Put plainly,

`what is the probability the project will resolve and release a fix when needed in a timely fashion?`

that you as a consuming can rely on in your own software supply-chain stream.  And even if you've assumed some responsiblity for resolving the issue and making it available upstream (in a PR/MR sort of way) will the project include the required changes and release for your downstream consumption?  Continuing with the thought exercise, even if you've decided to assume all maintenance responsibility and have forked the repository for your consumption - are you in check with their updates?  Meaning if someone reported a vulnerability and it gets resolved upstream does your supply-chain have an ability to consume those changes into your fork?  Of course, that inherent benefit of consuming someone's upstream solution into yours could be very powerful in terms of you delivery value in a cost-effective way.  But are you prepared for the inevitability of failures somewhere in that supply-chain.  And does having a pathway to paying for maintenance actually provide the ability for maintainers to affect change on those upstream dependencies?

## Developers have a Problem

The idea of [Shifting Security Left](https://cloud.google.com/solutions/shifting-left-on-security) has been a brewing topic recently - the basic idea that developers _should_ take care and put in due diligence at the source code development task, versus waiting for tests and scanners to evaluate (likely in a CI/CD pipeline).  This is complicated by the costs of tools/scanners, and how they are executed.  But even so, the focus is a bit misplaced and creates the issue of where developers need to invest skills.  I think most would agree development and software engineering is a skill requiring depth, and the more breadth required the more diluted the focus - and to include the focus on solving business problems (over coding).  But even with tools to assist developers in the evaluation of supply-chain dependencies - the process is more about risk management, prevention and mitigation, and improved decision making.  I am talking about due diligence at the design, architecture and decisions about stack and technology inclusions.  The issue become multi-directional: one major issue is that development generally happens once where developers make a decision about building versus consuming and then they move one; and another majore issue is that there is little formality or ceremony validating the decision (most likely if anything were to happen it would be during code-review at a PR/MR step).  Some teams utilize architectural decision records, and some use SBOMs to capture the decision to include a dependency - still that's one-shot and is static from the point of decision on.

## Due Diligence

As a software developer, I am generally lazy.  I want to spend the least amount of time possible on any one function - because I'm beholden to an estimation or some scope of delivery.  I just don't have time to put in the due diligence at depth for any dependencies.  I am positive it is the same for most developers, we simply want to deliver functionality.

A quick hypothetical - I know I need to include a logging library into my application.  So I do the obligatory search and find a handful of options.  If there is one associated by a supporting foundation I tend to have faith in its quality based on incubation requirements of that organization.  If not, I'll generally look for its source repo, and ask a couple basic questions: is there current/recent activity; are there more than one contributors; and, are there "stars", indicating at a minimum that others are watching this project.  I'll make a decision and add the dependency to my project's manifest and move on and integrate against it.


