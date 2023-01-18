---
layout: post
title:  "LowEndInsight Configuration Details"
date:   2023-01-04 10:46:41 -0500
categories: lei
---

## "So What is this Insight?"

LowEndInsight (LEI) looks into a git repo's details and infers some notion of risk for a couple key metrics:

* **Functional Contributors** - will detail this in a near-future post, but basically the select group of focus providing the most quantitative contributions to a given repo.  The gist is that we've found that most repos get the most contributions (commits, etc.) from a very small group, e.g., a repo that has 100 different contributions will boil down to 90+% of the commits come from one or two people.  Judging a project/repo by its number of contributors is misleading.
  
* **Contribution Currency** - the basic idea that a repo with recent activity is a positive indicator, or really a repo with no relatively recent activity is a negative indicator.  The inferred value of risk has more to do with -> what's the likelihood of the project getting updated if there is a known serious issue or security vulnerability.

* **Recent Commit Change** - looks at how "volatile" a project is.  This one isn't super interesting, unless there is actually a high percentage of change in recent commits.  Again the inference is about stability and a projects ability to respond to change.  For projects managed by Open Source foundations, this isn't likely to be an issue as they enforce incubation status and make it known that a project is likely to be more volatile.  However, for itch-scrathers, or hobby repos, this could be an indicator that the library or tool should be looked at closer before creating a downstream dependency.

There are other things LEI looks at, like the overall number of files and identifying any that are binaries.  It also looks for a standard SBOM file (i.e. bom.xml) in the repo.  If there is one present that means that the caretaker(s) are interested in support software supply-chain requirements for consumers.

More details in the LEI libraries README here: [https://github.com/gtri/lowendinsight/blob/develop/README.md](https://github.com/gtri/lowendinsight/blob/develop/README.md)


## "OK, So How Does This Map to Risk?"

The API running [here](https://rapidapi.com/quency-ai-quency-ai-default/api/lowendinsight2/) is configured this way:

``` elixir
config :lowendinsight,
  ## SBOM manifest risk  - when not present
  sbom_risk_level: System.get_env("LEI_SBOM_RISK_LEVEL") || "medium",

  ## Contributor in terms of discrete users
  ## NOTE: this currently doesn't discern same user with different email
  critical_contributor_level:
    String.to_integer(System.get_env("LEI_CRITICAL_CONTRIBUTOR_LEVEL") || "2"),
  high_contributor_level: System.get_env("LEI_HIGH_CONTRIBUTOR_LEVEL") || 3,
  medium_contributor_level: System.get_env("LEI_CRITICAL_CONTRIBUTOR_LEVEL") || 5,

  ## Commit currency in weeks - is the project active.  This by itself
  ## may not indicate anything other than the repo is stable. The reason
  ## we're reporting it is relative to the likelihood vulnerabilities
  ## getting fix in a timely manner
  critical_currency_level:
    String.to_integer(System.get_env("LEI_CRITICAL_CURRENCY_LEVEL") || "104"),
  high_currency_level: String.to_integer(System.get_env("LEI_HIGH_CURRENCY_LEVEL") || "52"),
  medium_currency_level: String.to_integer(System.get_env("LEI_MEDIUM_CURRENCY_LEVEL") || "26"),

  ## Percentage of changes to repo in recent commit - is the codebase
  ## volatile in terms of quantity of source being changed
  critical_large_commit_level:
    String.to_float(System.get_env("LEI_CRITICAL_LARGE_COMMIT_LEVEL") || "0.40"),
  high_large_commit_level:
    String.to_float(System.get_env("LEI_HIGH_LARGE_COMMIT_LEVEL") || "0.30"),
  medium_large_commit_level:
    String.to_float(System.get_env("LEI_MEDIUM_LARGE_COMMIT_LEVEL") || "0.20"),

  ## Bell curve contributions - if there are 30 contributors
  ## but 90% of the contributions are from 2...
  critical_functional_contributors_level:
    String.to_integer(System.get_env("LEI_CRITICAL_FUNCTIONAL_CONTRIBUTORS_LEVEL") || "2"),
  high_functional_contributors_level:
    String.to_integer(System.get_env("LEI_HIGH_FUNCTIONAL_CONTRIBUTORS_LEVEL") || "3"),
  medium_functional_contributors_level:
    String.to_integer(System.get_env("LEI_MEDIUM_FUNCTIONAL_CONTRIBUTORS_LEVEL") || "5")
```

Which is seen in the API response as:


``` json
"config": {
    "sbom_risk_level": "medium",
    "medium_large_commit_level": 0.2,
    "medium_functional_contributors_level": 5,
    "medium_currency_level": 26,
    "medium_contributor_level": 5,
    "high_large_commit_level": 0.3,
    "high_functional_contributors_level": 3,
    "high_currency_level": 52,
    "high_contributor_level": 3,
    "critical_large_commit_level": 0.4,
    "critical_functional_contributors_level": 2,
    "critical_currency_level": 104,
    "critical_contributor_level": 2,
    "base_temp_dir": null
}
```

If you run your own server - you can configure the settings however you wish. :smirk: