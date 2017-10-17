+++
author = "Chris Wolfe"
date = "2016-09-26T21:34:52-05:00"
description = "A small status aggregation system"
tags = ["clojure", "manifold", "side-project"]
title = "firedamp"

+++

[Firedamp](https://firedamp.herokuapp.com/) (source
on [github](https://github.com/derwolfe/firedamp)) is clojure application that
has been a blast to write. It is a simple application that is meant to be a cheap
stand-in for [statuspage.io](https://www.statuspage.io/), which is an _excellent_
service in itself.

Firedamp exists to fill a gap in some metrics that I found. I've frequently noticed
that build times might be dragging with travis, codecov, and github. The goal is
for this service to provide a single location that aggregates the simple human readable
status messages for all of these services.

The architecture of the application is simple and uses some clojure libraries that I've
come to really enjoy: ring, compojure, manifold, and aleph.
