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

The idea of [Shifting Security Left](https://cloud.google.com/solutions/shifting-left-on-security) has been a brewing topic recently - the basic idea that developers _should_ take care and put in due diligence at the source code development task, versus waiting for tests and scanners to evaluate (likely in a CI/CD pipeline).  This is complicated by the costs of tools/scanners, and how they are executed.  But even so, the focus is a bit misplaced and creates the issue of where developers need to invest skills.  I think most would agree development and software engineering is a skill requiring depth, and the more breadth required the more diluted the focus - and to include the focus on solving business problems (over coding).  But even with tools to assist developers in the evaluation of supply-chain dependencies - the process is more about risk management, prevention and mitigation, and improved decision making.  I am talking about due diligence at the design, architecture and decisions about stack and technology inclusions.  The issues become multi-directional: one major challenge is that development/implementation generally happens once where developers make a decision about building versus consuming and then they move on; and another pitfall is that there is little formality or ceremony in validating the decision (most likely if anything were to happen it would be during code-review at a PR/MR step).  Some teams utilize architectural decision records, and some use SBOMs to capture the decision to include a dependency - still that's one-shot and is static from the point of decision on.

## Due Diligence

As a software developer, I am generally lazy.  I want to spend the least amount of time possible on any one function - because I'm beholden to an estimation or some scope of delivery.  I just don't have time to put in the due diligence at depth for any dependencies.  I am positive it is the same for most developers, we simply want to deliver functionality. This is the shift-left dilemma. While I am generally in favor of developer responsibility extending from design to delivery, the scope issues associated with technical depth quickly become product/project debt.

> A quick hypothetical - I know I need to include a logging library into my application.  So I do the obligatory search and find a handful of options.  If there is one associated by a supporting foundation I tend to have faith in its quality based on incubation requirements of that organization.  If not, I'll generally look for its source repo, and ask a couple basic questions: is there current/recent activity; are there more than one contributors; and, are there "stars", indicating at a minimum that others are watching this project.  I'll make a decision and add the dependency to my project's manifest and move on and integrate against it.

There be dragons in that cave.  More often than not they don't bite, but that is a pure qualitative assumption.  The reality is that due diligence needs to be more acute, and to the point of consumption obligation to the end-deliverable supply-chain requirements (and noted accurately in a SBOM).

## The Contributor Myth

If we are employing some level of evaluation of upstream source code bases, and their historical information we tend to make the assumption that contributions and their currency are key indicators of the repository's health - the more contributions the better, and the more recent the contributions the better.  What we don't look at is the actual contributions and who they are coming from.

To tie-back into the shifting-left of security I believe it is fair to say that any eyeball test of a repositories health is better than none, and it is important to note that the depth of evaluation should depend on the downstream consumer's tolerance for risk.  For many products/projects this quick evaluation is probably enough.  However, it is also important to note that this level of security extends beyond the known vulnerabilities chase - mainly for two reasons: first is that vulnerabilities are rarely reported against low-level libraries and packages, and second is that most dependency "watchers" are at best telling you to stay current (e.g., tools like GitHub's dependabot).

Some quick introspection within a repositories `git` history reveals a bit more insight, possibly helping us answer the question above - what is the likelihood that the dependency will fix and release against an issue in a manner that reduces security risk?  First, we need to establish that not all contributions nor contributors are equal.  While all contributions are worthy and important to a repository they surely don't carry the same weight.

I am going to caveat here, that any project with less than two or three developers contributing to the repository is an indicator of risk, and consumers are assuming ownership of their dependence.  And this holds even more truth if that project does not belong to a foundational organization.  The absolute here is that unless there is a contractual relationship that would enforce some notion of maintenance on the upstream dependency, there is no guarantee those contributors will continue to support it, never mind that lacking a service-level agreement not timeline could be upheld.

### Functional Contributors

By looking into the history of a repository we can quickly see that the distribution of contributions reveals a subset of contributions as being key, or at least a majority belonging to a small grouping of contributors.  For the sake of this essay, let us call these functional contributors. For example, it is likely that given a 100 contributors to a repository, 90+% of them are attributed to a one or two developers.

And perhaps more important to the notion of risk management, we can also look at the timeline of the contributions and determine that they are more often than not on the front-end of the lifeline.

Here's some Jupyter/Python insight based on the analysis done by LowEndInsight on the [Python Black repo](https://github.com/psf/black):

```python
RK = os.environ.get('RAPID_KEY')

!{sys.executable} -m pip install requests pandas numpy scipy plotly chart_studio
import requests
payload = '''{"urls": ["https://github.com/psf/black"]}'''
headers = {'X-RapidApi-Key': RK, 'X-RapidApi-Host': 'lowendinsight2.p.rapidapi.com', 'Content-Type': 'application/json'}
r = requests.post('https://lowendinsight2.p.rapidapi.com/analyze/', data=payload, headers=headers)

import json
import time

j = json.loads(r.content)
print(j)
while j["state"] == "incomplete":
  r = requests.get("https://lowendinsight2.p.rapidapi.com/analyze/{}".format(j["uuid"]))
  print(r)
  j = json.loads(r.content)
  time.sleep(2)
  print("not complete yet, sleeping...")
```
    {'uuid': 'a59b9b78-8252-11ed-ad3e-be6b9608d8bb', 'state': 'complete', 'report': {'repos': [{'header': {'uuid': '90e36eea-8252-11ed-83db-0a70d9954673', 'start_time': '2022-12-22T23:44:22.130771Z', 'source_client': 'lei_worker', 'repo': 'https://github.com/psf/black', 'library_version': '0.7.3', 'end_time': '2022-12-22T23:44:24.810271Z', 'duration': 2}, 'data': {'risk': 'medium', 'results': {'top10_contributors': [{'name': 'Łukasz Langa', 'merges': 0, 'email': 'lukasz@langa.pl', 'contributions': 372}, {'name': 'Richard Si', 'merges': 0, 'email': '63936253+ichard26@users.noreply.github.com', 'contributions': 123}, {'name': 'Jelle Zijlstra', 'merges': 0, 'email': 'jelle.zijlstra@gmail.com', 'contributions': 109}, {'name': 'Zsolt Dollenstein', 'merges': 1, 'email': 'zsol.zsol@gmail.com', 'contributions': 79}, {'name': 'dependabot[bot]', 'merges': 0, 'email': '49699333+dependabot[bot]@users.noreply.github.com', 'contributions': 52}, {'name': 'Hugo van Kemenade', 'merges': 0, 'email': 'hugovk@users.noreply.github.com', 'contributions': 47}, {'name': 'Cooper Ry Lees', 'merges': 0, 'email': 'me@cooperlees.com', 'contributions': 43}, {'name': 'Felix Hildén', 'merges': 0, 'email': 'felix.hilden@gmail.com', 'contributions': 25}, {'name': 'Batuhan Taskaya', 'merges': 0, 'email': 'isidentical@gmail.com', 'contributions': 24}, {'name': 'Marco Edward Gorelli', 'merges': 0, 'email': 'marcogorelli@protonmail.com', 'contributions': 20}], 'sbom_risk': 'medium', 'recent_commit_size_in_percent_of_codebase': 0.00014, 'large_recent_commit_risk': 'low', 'functional_contributors_risk': 'low', 'functional_contributors': 31, 'functional_contributor_names': ['Łukasz Langa <lukasz@langa.pl>', 'Hugo van Kemenade <hugovk@users.noreply.github.com>', 'Yilei "Dolee" Yang <yileiyang@google.com>', 'Batuhan Taskaya <isidentical@gmail.com>', 'Joe Antonakakis <jma353@cornell.edu>', 'Richard Si <63936253+ichard26@users.noreply.github.com>', 'Marco Edward Gorelli <marcogorelli@protonmail.com>', 'Michael J. Sullivan <sully@msully.net>', 'Felix Hildén <felix.hilden@gmail.com>', 'Nipunn Koorapati <nipunn1313@gmail.com>', 'Carol Willing <carolcode@willingconsulting.com>', 'dependabot[bot] <49699333+dependabot[bot]@users.noreply.github.com>', 'Shantanu <12621235+hauntsaninja@users.noreply.github.com>', 'Jon Dufresne <jon.dufresne@gmail.com>', 'Joseph Young <80432516+jpy-git@users.noreply.github.com>', 'Jelle Zijlstra <jelle.zijlstra@gmail.com>', 'Christian Heimes <christian@python.org>', 'Bryan Bugyi <bryanbugyi34@gmail.com>', 'Zsolt Dollenstein <zsol.zsol@gmail.com>', 'hauntsaninja <hauntsaninja@users.noreply.github.com>', 'Cooper Ry Lees <me@cooperlees.com>', 'Jakub Kuczys <6032823+jack1142@users.noreply.github.com>', 'James Addison <jay@jp-hosting.net>', 'Anthony Sottile <asottile@umich.edu>', 'Antonio Ossa-Guerra <aaossa@uc.cl>', 'jgirardet <ijkl@netc.fr>', 'Shivansh-007 <shivansh-007@outlook.com>', 'Cooper Lees <cooper@fb.com>', 'David Szotten <davidszotten@gmail.com>', 'Sagi Shadur <saroad2@gmail.com>', 'Yurii Karabas <1998uriyyo@gmail.com>'], 'contributor_risk': 'low', 'contributor_count': 359, 'commit_currency_weeks': 0, 'commit_currency_risk': 'low'}, 'repo_size': '0', 'repo': 'https://github.com/psf/black', 'project_types': {'python': ['/tmp/lei-1671752662-85-hk5i99/black/docs/requirements.txt', '/tmp/lei-1671752662-85-hk5i99/black/test_requirements.txt']}, 'git': {'hash': '3246df89d6d80fc09357b445630fad87f08f57ce', 'default_branch': 'refs/remotes/origin/main'}, 'config': {'sbom_risk_level': 'medium', 'medium_large_commit_level': 0.2, 'medium_functional_contributors_level': 5, 'medium_currency_level': 26, 'medium_contributor_level': 5, 'high_large_commit_level': 0.3, 'high_functional_contributors_level': 3, 'high_currency_level': 52, 'high_contributor_level': 3, 'critical_large_commit_level': 0.4, 'critical_functional_contributors_level': 2, 'critical_currency_level': 104, 'critical_contributor_level': 2, 'base_temp_dir': '/tmp'}}}]}, 'metadata': {'times': {'start_time': '2022-12-22T23:44:59.571782Z', 'end_time': '2022-12-22T23:44:59.577293Z', 'duration': 0}}}



```python
results = j["report"]["repos"][0]["data"]["results"]
#print(results)
top10 = results["top10_contributors"]
#print(top10)

for c in top10:
    print("{} [{}]: {}".format(c["name"], c["email"], c["contributions"]))

import pandas as pd
import numpy as np
import scipy as sp
import plotly
plotly.offline.init_notebook_mode(connected=True)
import plotly.figure_factory as ff

df = pd.DataFrame.from_dict(top10)
table = ff.create_table(df)

#plotly.offline.plot(table, filename='lei-table1.png')
table.show()
```

    Łukasz Langa [lukasz@langa.pl]: 372
    Richard Si [63936253+ichard26@users.noreply.github.com]: 123
    Jelle Zijlstra [jelle.zijlstra@gmail.com]: 109
    Zsolt Dollenstein [zsol.zsol@gmail.com]: 79
    dependabot[bot] [49699333+dependabot[bot]@users.noreply.github.com]: 52
    Hugo van Kemenade [hugovk@users.noreply.github.com]: 47
    Cooper Ry Lees [me@cooperlees.com]: 43
    Felix Hildén [felix.hilden@gmail.com]: 25
    Batuhan Taskaya [isidentical@gmail.com]: 24
    Marco Edward Gorelli [marcogorelli@protonmail.com]: 20

![table](../assets/images/newplot.png)

```python
import plotly.express as px
fig = px.bar(x=df['name'].to_list(), y=df['contributions'].to_numpy())
fig.show()
```

![plotly.express](../assets/images/lei-bars.png)

At first glance this doesn't look like a terrible distribution.  Remove the `dependabot` contributions and it still isn't bad.  Looking a little closer - we can see the that `Langa` has the majority of contributions, however they have not contributed in over a year.  Still, it looks like a fairly low-risk project.  Aside from Black being an SCA tool I think it highlights what a fairly healthy repo looks like.

Let's take a look at [ExpressJS/express](https://github.com/expressjs/express):

![lei-express-table](../assets/images/lei-express-table.png)

It becomes clear that this distribution looks considerably different - with the majority of contributions coming early in the project's history.

![lei-express-plot](../assets/images/lei-express-plot.png)

This isn't earth shattering knowledge, however it simply shows that contributions in terms of quantity can be mapped to a timeline and distribution making it easier to gain useful insight into a source repository.  This metadata can be used to informed a decision maker - quantifying the "eyeball" test.  In the context of due diligence if you add this insight to the fact that `express` has almost 60K stars on GitHub one can begin to paint a better picture of assumed risk.
