+++
title = "Use Private Repos in requirements.txt"
description = "Stop googling this and getting the wrong answer"
author = ""
date = 2018-02-20T09:44:44-06:00
draft = false
+++

I have had to Google this constantly and am tired of doing so. I regularly work with private Github repositories and from time to time want to pull down PRs and test them out as part of an integration test. I do this by pulling down the original branch and using `pip` to install it. The following line is what should appear in your `requirements.txt` file:

```
git+ssh://git@github.com/<fork>/<project>@<branch>#egg=<project>
```

- `fork` would be something like `derwolfe` (my github account)
- `project` would be the actual name of the project, like `twisted`.
- `branch` is the branch used for the PR.
