+++
title: "Manifold streams, testing, no restarts"
description = "Using lein auto test and try-take to stop wasting time"
author = "Chris Wolfe"
date: 2017-10-16T20:57:06-05:00
draft: true
+++

I've been using Clojure for about two years. Much of that time has been spent using Zach Tellman's [Manifold](https://github.com/ztellman/manifold) library that makes it easier to build event-driven systems. I also tend to prefer a test driven approach to development. This means that I tests that are deterministic and quick to fail or succeed.

Much of the Clojure code that I write for network based systems ends up using manifold streams to build processing pipelines. This means that my tests frequently have some assertions that require that I extract a value from a stream or push a value onto one.

One of the slightly frustrating things about Clojure when coming from a language like python is how long it takes for the JVM to start up and for leiningen to be ready to run tests. This can be mitigated by using something like [lein auto](https://github.com/weavejester/lein-auto). It keeps the JVM up, reloads your code, and can rerun your tests. This is extremely helpful _assuming you haven't written code that blocks forever_.

Essentially, I have accidentally written part of a test backwards - don't run this unless you mind needing to restart your repl.

```clj
(require '[clojure.test :refer :all])
(require '[manifold.stream :as ms])

(deftest example-test-blocks-forever
  (let [a (ms/stream)]
    (is (= 1 @(ms/take! a)))))

(run-tests)
```

The annoying thing about this code is that the assertion step will never complete, the test will not timeout, and to continue developing, I'll need to restart my test runner/JVM.

There are several ways that I could have avoided this:

1. Pay better attention when writing my test. Sure, but let's assume I didn't.
2. Add a way to timeout an entire test (this is what [twisted](http://twistedmatrix.com/documents/current/api/twisted.trial.unittest.TestCase.html#timeout) does and I miss it).
3. Don't use function that can block indefinitely in the tests.

In the longer term, I'd like to write a function/test macro that will timeout a test after a specified number of seconds. But, as I don't have that right now, I have settled on using option #3.

```clj
(require '[clojure.test :refer :all])
(require '[manifold.stream :as ms])
(require '[manifold.time :as mt])

(deftest example-test-timeout
  (let [a (ms/stream)]
    (is (= 1 @(ms/try-take! a ::drained (mt/seconds 1) ::timeout)))))

(run-tests)
```

This works well enough, as long as a I remember to use it. The tests are deterministic as far as the streams are concerned. Using `try-put!` and `try-take!` with short timeouts makes the tests fail quickly and obviously - if the take or put hasn't succeed then either my assumptions or my implementation are wrong.
