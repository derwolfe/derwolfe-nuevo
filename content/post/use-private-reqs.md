+++
title = "Using private repos with requirements.txt"
description = "Google this and get the right answer"
author = ""
date = 2018-02-20T09:44:44-06:00
draft = false
tags = ["python", "packaging", "private repositories", "pip", "requirements.txt"]
+++

I regularly work with private Github repositories and from time to time want to pull down PRs and test them out as part of an integration test. I do this by pulling down the original branch and using `pip` to install it. The following line is what should appear in your `requirements.txt` file to make this dream a reality:

```
git+ssh://git@github.com/<fork>/<project>@<branch>#egg=<project>
```

- `fork` is the account holder of the fork, normally a github username, e.g. `derwolfe`
- `project` is the actual name of the project, e.g. `twisted`
- `branch` is the branch used for the PR
