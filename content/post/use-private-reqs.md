+++
title = "Using private repos with requirements.txt"
description = "Ever wanted to test a github PR as a part of an integration test?"
author = ""
date = 2018-02-20T09:44:44-06:00
draft = false
tags = ["python", "packaging", "private repositories", "pip", "requirements.txt"]
+++

I regularly work with private Github repositories and from time to time want to pull down PRs and test them out as part of an integration test. I've had to Google the right syntax for this many times. No more.

To install a branch from a private repository in a project using `requirements.txt` files, replace the line containing the actual requirement with the following:

```
git+ssh://git@github.com/<fork>/<project>@<branch>#egg=<project>
```

- `fork` is the account holder of the fork, normally a github username, e.g., `derwolfe`
- `project` is the actual name of the project, e.g., `twisted`
- `branch` is the branch used for the PR, e.g., `rewire-reactor-1231`
