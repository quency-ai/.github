---
layout: post
title:  "Yak-Shaving in Paradise, The Vulnerabilty versus Prevention Chase."
date:   2022-12-23 10:46:41 -0500
categories: lei
---

## "Here Be Dragons"

Caveat: this is not a formal essay, instead is a stream of thought inspired by general holiday-ish boredom.  Apologies in advanced if you've found yourself wanting your time back after skimming.  And so it is just my opinion, based on many years of experience in specific software engineering realms and at various organizations.  I've also put down some basic research for sponsors who wish to remain unknown. This is a long read, and I promise no guarentee of anything more than maybe a head-nod or two, and some free entertainment if you're willing.

No editor was wasted in the production of this, apologies for the write-to-post lack of quality.

### TLDR;

* Known vulnerabilities are a lagging indicator of security, never-ending tail-chasing.  Just because you think you have none doesn't mean you are secure (at all)
* MTTR is the biggest vulnerability of them all, if you have manual processes in place to update your system - you're effed (simply because you'll be exposed longer than you will be secure)
* Developers make uninformed decisions, and it isn't their fault, feature delivery is often more important than evaluating the consequences of a bad decision
* Accountability, and the lack of it, is a root cause - developers move on, who's actually responsible for all ZeroTrust
* There are tools, but are they actually helping, based on the number of publicly-known exploits we should be trying to determine this
* Prevention is probably cheaper, so why the focus on ops-based security in the software engineering space still
* This guy is crazy, and this is real long
* Building, hosting and continuously delivering functionality is really hard

---
## "Bus-factor Critical"

![LEI Bus Factor Bus](https://raw.githubusercontent.com/gtri/lowendinsight/develop/lei_bus_128.png)

The risk with picking up an old pet project, well, is that reason why it is an "old" pet project.  For whatever reason(s) the effort didn't elevate beyond an exercise in learning.  This is a not-so-short story.

Circumstances led me back to the core idea that the biggest vulnerability in software is how long it takes to update it - in the event that another vulnerability becomes know, or the system is exploited.  For most organizations "pulling the plug", and preventing operations until a "clean" system can be rebuilt, isn't a realistic option (though in many cases that simply the only way to be sure you've restored completely).  While there is an ecosystem of site-reliability focus, there are whole systems that are simply not only web-based, but logically distributed across the globe in various types of compute, storage and networks.  While cyber ops work to protect/attack the forts everywhere software engineers continue to build systems that lag, unprepared for the operational future they target.  It isn't just because "software is eating the world", but also the scope and rate of change that is happening within each piece of software (components that exponentially compound).

I don't know what the solution is.  But I do know this...amongst all the hypewagons (e.g., software supply-chain, ZeroTrust, DevSecOps, shifting security left, Continuous Delivery, vulnerability this, vulnerability that, and on and on) nothing is actually effecting the risk reduction needed at the very genesis of the source development/coding process - specifically when a developer or team decides to include upstream code into their project, aside from soft process to review the change.  I've known good teams to include Architectural Decision Records (ADRs) into the PR/MR process, and include them in code reviews.  But here is the core, root of all symptoms, problem: there are more not-good teams than good ones - and in most cases it is not the teams fault, but the system or organanizational demands that drive for velocity over quality and performance.  This also includes the misuse of Agile methods as a input to bad assumptions.  Software Engineering, unlike its contemporary engineering counterparts lacks the professional accountability needed to ensure the appropriate due diligence, based on accepted risk tolerances, is actually completed as part of the code development, and dependency selection and inclusion process.

### Metaphorically...
![Pencil Manufacturing](https://blog.pencils.com/wp-content/uploads/2012/09/block_pencil_progression1.jpg)

source: https://blog.pencils.com/pencil-making-today-2/

Take something simple, like the manufacturing of a pencil, and imagine the amount of engineering rigor that goes into the design, the selection and sourcing of raw materials - all the way through to the quality-control checks that happen pre and post packaging and delivery.  Plus, all of the provenance material that exists to trace from end-customer back to raw-material sources in the event of an identified lack of quality.  Each raw material (for pencils there are but four), is meticulously evaluated against acceptance (read risk) criteria, and managed within an audited inventory system.  Hopefully that scene is clear in your mind.  Now considered that there is none of that for software - even in many "critical" systems.  Safety critical systems have special formalities, such as DO-178C, that describe the requisite processes and checks for software.  In addition there are requirements for 3rd-party reviews of documentation and analysis of the delivery infrastructure.  We all know there is a cost-value equation that underlies this dilmemma for software and its most cases risk IS managed accordingly.

So back to the root issue - the accountability constructs simply don't match the actual risk, because there is no demand for software engineering entities to prove their proverbial metal.  Even with maturity models such as CMMI and CMMC, the focused energy is on scanning and monitoring - over prevention.  I can't help but want to return to that old pet project to try and grow the thinking around how to prevent developers and teams from making bad decisions about other people's source code that they include into their projects.

## Yak-Shaving, Paradise Maybe...

My pet-project, really basic analysis and a search for low-level insight, has led my on one of the biggest yak-shaving exercises I've ever experiences in a technology project.  While LowEndInsight started as an academic research exercise, the basic hypothesis that an Open Source project's main risk isn't the code itself, but that bad assumption that there are actually maintainers sustaining the life of it, while you as a developer/end-user consume it, I (and we) have learned that it is just a tad more complicated than sustainment.  The package, the binary, or however the source is delivered to your consumption loading-dock is a very large attack vector itself - that the software supply-chain is a very weak link in the delivery apparatus.  

``` sh
✗ mix lei.analyze https://github.com/facebook/react | jq
{
  "state": "complete",
  "report": {
    "uuid": "4d1e2b08-7b68-11ea-9ca1-88e9fe666193",
```

When we started with LowEndInsight we were working towards evaluating every package in a select set of package ecosystems (e.g. PyPi, NPM, Hex, Maven, and a few others), looking to trace from an available package, back to its source and build environment - looking for provenance and provability that the delivered package is actually the same as the one built.  There was other research about that navigation that I can't share, but hopefully it makes a little bit of sense.  Regardless, we quickly found that none of the package managers actually had any provability built in, and almost none of them even provided a way to automatically (no humans) go from a package in a list to the source repo.  While some do have the field in their publication API, there isn't any enforcement or validation of the mapping to the source - so developers could input anything, legit or otherwise.  As you could imagine this kind of distributed processing required mass amount of compute resources and some basic intelligent design to handle the serial processing of APIs, from the package manager to the source repo.  NPM alone is well into the many millions of packages and we couldn't afford to wait the long times for the snake to navigate and collect the source, before beginning our own analysis on the repos themselves.  

Thus we met the Yak.

## Not All Contributions in Open Source are Equal

Durr.

Not only are not all contributions equal, neither are all contributors equal.

Durr durr.

"Bus factor" is a silly term - sparkin' the idea that there is some risk associated with a single entity holding all, or key, knowledge.  The risk revolving around what happens if that entity gets "hit by a bus".  Generally its a scary thought.

One of the first "metrics" we got after with LowEndInsight was tracking the number of contributions per contributor in a given repo.  Another tendril of the yak-shaving experiment was a decision to limit our data set to the raw git repo (public ones at that) itself - avoiding the complexities of managing n-keys to all the different "hubs" hosting git repos (e.g., GitHub, Bitbucket, and GitLab).  One of the core values of a distributed SCM capability is the self-contained history - so we knew if we could pull the repo, we could access it's entire "public" history.  There is a challenge with forks and other repo histories, but we ultimately determined that to be noise in the grand scheme - it is something I'd like to get back to and see how possible it would be to confirm the lifelong of a repo's history, looking for anomolies that could be categorized accurately.  We also found some basic data cleaning challenges - for example, it is quite possible that a single contributor could have "committed" changes to a repo, using a different ID (user name and email combo). Nevermind that this "registration" isn't validated - there's no actual real authenticity check there, it's a bit of information the developer configures on the machine where the git commit happens.  We also quickly realized that most "managed" open source repos use bots to do various things on the repo, and so there were quite a few contributions coming from non-humans.  One of the more regular sightings was Dependabot - a dependency-checker that would submit a PR to a repo, if there were a new version of a dependency available (and tested).  No proof, but I'm making the assumption that more often than not, if the tests passed - the repo/project owner would merge the changes in without a lot of review.

### Functional Contributors are the Ones that Matter

![Contributor Structure - Dictator/Lieutenants](https://git-scm.com/book/en/v2/images/benevolent-dictator.png)

source: https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows

We decided to categorize contributors based on the number of the their contributions - basic stuff.  In hindsight, when the contributions happen, and the content of the contribution to the repo matters too.  For example, if the lion's share of a contributor's contributions come at the beginning of the life of the repo, but then tail off - perhaps they are no longer a functional/functioning contributor.  Another yak-shaving exercise begins here.

Fortunately, it's not too difficult to calculate the collection of contributions per a user's provided name and email (again, these are arbitrary details that developer configure into their git client).

``` elixir
  @spec get_functional_contributors(Git.Repository.t()) :: {:ok, non_neg_integer, [any]}
  def get_functional_contributors(repo) do
    {:ok, counts, total} = get_contributor_distribution(repo)
    {:ok, length, filtered_list} = GitHelper.get_filtered_contributor_count(counts, total)
    {:ok, length, Enum.map(filtered_list, fn {name, _value} -> name end)}
  end
```

(source is here if you want to follow the functions lower: https://github.com/gtri/lowendinsight/blob/develop/lib/git_module.ex#L244)

LowEndInsight was built to report this information as JSON, here's a quick look at a "top 10":

``` json
{
    "top10_contributors": [
        {
            "name": "Paul O’Shannessy",
            "merges": 959,
            "email": "paul@oshannessy.com",
            "contributions": 1777
        },
        {
            "name": "Dan Abramov",
            "merges": 86,
            "email": "dan.abramov@gmail.com",
            "contributions": 1356
        },
        {
            "name": "Sophie Alpert",
            "merges": 392,
            "email": "git@sophiebits.com",
            "contributions": 1265
        },
        {
            "name": "Brian Vaughn",
            "merges": 101,
            "email": "bvaughn@fb.com",
            "contributions": 995
        },
        {
            "name": "Sebastian Markbåge",
            "merges": 141,
            "email": "sebastian@calyptus.eu",
            "contributions": 803
        },
        {
            "name": "Jim Sproch",
            "merges": 327,
            "email": "jsproch@fb.com",
            "contributions": 456
        },
        {
            "name": "Brian Vaughn",
            "merges": 65,
            "email": "brian.david.vaughn@gmail.com",
            "contributions": 363
        },
        {
            "name": "Dominic Gannaway",
            "merges": 6,
            "email": "trueadm@users.noreply.github.com",
            "contributions": 336
        },
        {
            "name": "Pete Hunt",
            "merges": 126,
            "email": "floydophone@gmail.com",
            "contributions": 332
        },
        {
            "name": "Andrew Clark",
            "merges": 2,
            "email": "acdlite@fb.com",
            "contributions": 264
        }
    ]
}
```

Hopefully, this gives a bit of insight into that particular repo, and where we were headed.

**The question for me now, and the basic purpose of picking up LowEndInsight again, is how do we turn that raw data into useful information for developers - at the time of the decision to include it - before it is added to a work-in-progress' manifest of dependencies - where the focus turns to chasing "known vulnerabilities".  What is the likelihood that if (big if) there ever is a vulnerability reported against some OSS project, that it will actually be fixed and delivered downstream?  And if you had some notion of that likelihood what would a developer do with it given their focus is on developing and delivery some feature in a system?  There is an incredible amount of unvalidated trust built-in to this relationship.**

## There is a Lot of Molecular Gyration

SCA isn't new, but is still immature.

Of course LowEndInsight was not the first look at this stuff - if you search for Bus-Factor or software supply-chain you'll find that there are many tools commercially available to track vulnerabilities in dependencies.  This is great, realization that the transitive dependency matrix is an extremely volatile situation.  However, I'm still finding that almost all of these tools are focused on "scanning" an in-development repository, and not the upstream repos.  More importantly, these tools are also executing after the decision was made to include an upstream dependency - not before, not from a preventative perspective - not from within the decision-making process (like an ADR).

There are dedicated conferences to this junk:
- [https://www.ieee-scam.org/2022/](https://www.ieee-scam.org/2022/)
- [https://conf.researchr.org/home/msr-2023](https://conf.researchr.org/home/msr-2023)

There are products trying:
* [https://codescene.com/](https://codescene.com/)
* [https://bytesafe.dev/](https://bytesafe.dev/) 
* [https://tidelift.com](https://tidelift.com)
* many many others in proximity

There are other movements:
* Secure Coding Practices: [https://wiki.sei.cmu.edu/confluence/display/seccode/Top+10+Secure+Coding+Practices](https://wiki.sei.cmu.edu/confluence/display/seccode/Top+10+Secure+Coding+Practices)
* OWASP: [https://owasp.org/www-pdf-archive/OWASP_SCP_Quick_Reference_Guide_v2.pdf](https://owasp.org/www-pdf-archive/OWASP_SCP_Quick_Reference_Guide_v2.pdf)
* [https://minimumcd.org](https://minimumcd.org)
* Executive Orders: [https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)
  
And others ranting:
* [https://blog.crashoverride.com/cve-nvd-doesnt-work-for-open-source-and-supply-chain-security](https://blog.crashoverride.com/cve-nvd-doesnt-work-for-open-source-and-supply-chain-security)
  
### Yeah, So What
Ok, so we have some very basic insight, so what.  Do developers want another tool in their toolbox?  Do product owners/project managers?  Is this just an education process, does software engineering and system administration need another renaissance to grow it beyond the dysfunction of academia (i.e., computer science, IT, and cyber programming and degree-orientation), tool-specific certifications, and organizational mal-prioritization?  Is this a simple leadership error, and we're not creating the culture required to get us of top-dead-center here?

## Back to the Yak

Even if there is tooling available to assist in the decision making process, before dependencies are created, I'm not sure how that integrates into the current suite of development team operations - agile or not.  What does it mean to get a developer to rationalize functional contributors, and the quality of an open source project, as they make a decision.  Let's just say it were a simple query to find out, what would be next on this yak-shaving journey?  Hosting LowEndInsight at a publicly available endpoint?  Sure, but then LowEndInsight needs to be more than a library - executed from an Elixir/Erlang VM.

### Hosting

Back in the day I was a big fan of Heroku, and really still am.  But the economies of hosting have changed, and Heroku isn't the cheapest.  What are the options?  I started with Fly.io, and it is worth the look, but ran into some challenges with their support model which made it an ineffective right away.  While poking around at hosting a simple Elixir-based appication (sans Phoenix) I quickly realized I needed to be thinking about authentication/authorization and scaling.  And of course that let me to start thinking about what it would mean to monetize this interface - at a minimum to cover the costs of exposing it publicly.

Full disclosure at this point I'm not thinking too hard about the business case, only that I know there needs to be some thought put into this "prevention" angle.

Even from the early research-y days of LowEndInsight we knew that there were scaling challenges.  In our academic environment we had access to HPC, many, many cores where we could programatically distribute from a single supervisor process.  And Erlang's OTP - from Elixir - solved many of the underlying distribution and job process challenges.  From the Internet and an API perspective, things are a lot different.

### Asynchronous Handling of Work

Next stop on the yak-shaving train is figuring out how best to handle the long-running process of LowEndInsight's analysis which basically consists of receiving a URL to a git repo, cloning that repo, and running a suite of processes against that repo (for the most part serially still).  I quickly decided to separate the submission of the git repo URL(s) from the delivery of the resulting analysis/report - it was going to be a two-hit operation: POST the inputs and return a work ID, and GET the results by providing the ID.  I knew this would put some logic burden on the consumer, they'll now have to poll until that get a status "complete" indicator on the GET request.  There are technologies that _could_ be used to handle this better - and maybe that's a future experiment, specifically thinking about WebSockets.  Though I'm still only really working this from an API perspective.

This works currently:

``` python
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

Not the worst, and don't really want to optimize here yet.  Because...

### Caching Results AND Longtime Report Storing

Another challenge that we knew existed in our HPC work, was the need to "cache" analysis.  There were multiple drivers for this, but the biggest was in handling errors - we simply didn't want to have to start large-scale analysis runs over from scratch.

We also knew that our analysis was fairly temporal, Open Source repositories are living entities, potenially and constantly changing. The second we analyse a repo, our analysis _could_ be outdated.  After a brief discussion we decided on caching analysis for a repo for a 30-day window, before ejecting the analysis from our immediate data store (so that a subsequent request for a git repo, would trigger new analysis).  There are quite a few cache-oriented technologies out there and like most architectural decisions we focused on tools we already had some familiarity with.  It really came down to the simplicity of Redis, and ability to host a service just about anywhere with good resiliency.  We did consider using features of Erlang-OTP/Elixir but we believed the caching capability needed to stand alone and be somewhat abstracted from the larger infrastructure.

With Redis in hand, we lost site with the longer-term storage needs.  And it wasn't until I started thinking about publicly hosting the API, that I realized that Redis wasn't the answer.  My initial thoughts tilted towards PostgreSQL as a logical backend database for the service at large.  But first, back to the hosting saga.

### Hosting Part Two

The auth and throttling questions - how to ensure no one abuses the system with a kagillion queries that I'm paying for the processing of - sat heavy in my mind.  I'd initially started to build a front-end, "business", interface to allow registration, and potentially subscription to degrees of access.  What a PITA, handling credit cards complicates everything obviously, but simply creating the ability to create and manage a token to access the API with...ooof.

I'd asked the question a few times in various places - why isn't there an API-Front-End-as-a-Service (AFEaaS) out there already.  I wasn't pointed to a few things that were close, but not it.  I stewed on this for a while...maybe this is a biz opportunity (more on this thought line in a second).  Another random post in HackerNews referencing the need again, and I stumbled upon RapidAPI (now just Rapid).  Rapid appears to be an API brokering service - that enables B2B-ish transactions, including proxying API's hosted elsewhere.  I immediately signed up to investigate and determined it was worth a shot.

Back to where to host the actual API.  After struggling with Fly.io I asked some basic questions in various channels about hosting Elixir (non-Phoenix) apps in a GitOps-deployable way.  I was pointed in the direction of Render.com - alas another new hosting capability modeled after Heroku.  It took no time at all to get my basic API service hosted in Render, including a Redis cache service to support it.  I redirected my Postman environment to the Render endpoint and let loose a handful of API requests - and immediately saw the next sequence of yak-shaving efforts appear.  First, was scaling the request handler, second was scaling to support the in-memory handling of a larger-than-normal git repo (LowEndInsight's analysis does run against a bunch of files in the repo - more optimization needed there).  It was too easy to crash the very small shared VM that Render was providing for free.  Even with a dedicated VM, I was blowing out the unscaled machine with multiple repos - the HPC-based distribution across multiple cores was killing the available memory fast.  Even limited the request to one core and one process per core, it wasn't working without bonking on a larger git repo.

I was doomed to a vertical scaling challenge unless I separated the analysis out of the main process.  Fortunately this is super straightforward in Elixir.  But, as I started to shave in that direction I couldn't help but begin to notice the smell of a problem creating a new problem.  Having working on a handful of large-scale distributed worker challenges I knew that job-queue-worker environment would be a fairly simple way to get horizontal scaling that could be managed at the central queue (which I already had in place with Redis).  Another big pro to Render as a hosting solution was the ability to designate a service as a background worker, with no external interfaces (preventing that abuse/attack path).  The infrastructure landscape started to be come clear - an API service, a worker service that included the LowEndInsight library, and a little bit of glue to make the connection between Render and GitHub to get GitOps in full-effect.  This didn't take much time and I was able to get some stability in the API ops.  And the cache worked too. 

![LEI at Render](/assets/images/render-lei.png)

### Fronting an API with Rapid

The next trick, yak-shaving is as much about voodoo, pixie dust, and hope, was to get Rapid wired up.  While I had some OpenAPI/Swagger docs done, I realized I need to get them to some sort of complete to initiate the Rapid front-end based on the defined endpoints.  I will actually cut this story short here - it wasn't very straightforward.  Rapid, not sure of their legitimacy, seems to be working on version 2 of their biz.  There were multiple conflicting UIs, mis-mapped URLs, and not-so-obvious challenges, including waffling support channels.  It took a lot more work to get the two interfaces: an HTTP POST and GET, wired up and connected to my Render service.

Once it was basically working, I created a couple of fake users to test the Rapid auth structure against my defined subscriptions.  It worked, and I felt a bit of joy - for a second.  The reality of getting this thing this close to public consumption, beyond a basic proof-of-concept website created a raw sense of fear.  Being confronted with the why the hell was I doing this anyway, it almost cut my yak-shaving, dragon cave, escapade off at the knees.

![Rapid API LowEndInsight](/assets/images/rapidapi-lei.png)

## Reality Check

If anyone is actually reading my brain-dump here, well, thanks.

Now maybe you can help me answer the root question - how to get a developer the information needed to make a quality decision.  Functional contributions being but one indicator - LowEndInsight providing other bits like commit currency - how long since the last commit, or amount of change in recent commits - basic volitality.

A Jupyter Notebook plot of contributors example:

![jupyter Plot of LEI Analysis](/assets/images/newplot.png)

## Comments?

Dunno, not really sure.  Probably going to syndicate this to Medium.  But for posterity, if you do want to leave a comment or kick off a discussion feel free to do so [in this GitHub issue](https://github.com/quency-ai/.github/issues/1).  I know some people have wired up GH issues to the Jekyll site.  Mo yak-shaving...for another day.

