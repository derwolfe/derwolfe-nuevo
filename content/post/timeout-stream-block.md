+++
title = "Manifold streams, testing, no restarts"
description = "Using lein auto test and try-take to stop wasting time"
author = "Chris Wolfe"
date = 2017-10-16T20:57:06-05:00
draft = false
tags = ["clojure", "manifold", "streams", "testing"]
+++

I've been using Clojure for about two years. Much of that time has been spent using Zach Tellman's [Manifold](https://github.com/ztellman/manifold) library that makes it easier to build event-driven systems. I also tend to prefer a test driven approach to development. This means that I tests that are deterministic and quick to fail or succeed.

Much of the Clojure code that I write for network based systems ends up using manifold streams to build processing pipelines. This means that my tests frequently have some assertions that require that I extract a value from a stream or push a value onto one.

One of the slightly frustrating things about Clojure when coming from a language like python is how long it takes for the JVM to start up and for leiningen to be ready to run tests. This can be mitigated by using something like [lein auto](https://github.com/weavejester/lein-auto). It keeps the JVM up, reloads your code, and can rerun your tests. This is extremely helpful _assuming you haven't written code that blocks forever_.

Essentially, I have accidentally written part of a test backwards. For example, the following test will start once and block forever; `lein auto test` won't track further changes to it until the process is killed and restarted. Note, this isn't `lein auto test`'s fault, the test simply blocks forever due to a mistake I've made.

The source for the tests and its dependencies is available at [derwolfe/test-restarts-examples](https://github.com/derwolfe/test-restarts-examples).

```clj
(deftest ^:slow example-test-blocks-forever
  (let [a (ms/stream)]
    (is (= 1 @(ms/take! a)))))
```
Source for this test is [here](https://github.com/derwolfe/test-restarts-examples/blob/master/test/test_restarts/core_test.clj#L7-L9)

If you have pulled down the code mentioned above, you should be able to run `lein auto test :slow` to run this test. This function will never complete. The test will not timeout and to continue developing one would need to kill and restart the test runner/JVM.

So, how to avoid doing this in the future?

1. Pay better attention when writing my test. Sure, but let's assume I didn't.
2. Add a way to timeout an entire test (this is what [twisted](http://twistedmatrix.com/documents/current/api/twisted.trial.unittest.TestCase.html#timeout) does and I miss it).
3. Don't use function that can block indefinitely in the tests.

In the longer term, I'd like to write a function/test macro that will timeout a test after a specified number of seconds. But, as I don't have that right now, I have settled on using option #3.

```clj
(deftest example-test-timeout
  (let [a (ms/stream)]
    (is (= 1 @(ms/try-take! a ::drained (mt/seconds 1) ::timeout)))))
```
Source for this test is [here](https://github.com/derwolfe/test-restarts-examples/blob/master/test/test_restarts/core_test.clj#L11-L13)

This works well enough, as long as a I remember to use it. The tests are deterministic as far as the streams are concerned. Using `try-put!` and `try-take!` with short timeouts makes the tests fail quickly and obviously - if the take or put hasn't succeeded then either my assumptions or my implementation are wrong.

So, if you are using manifold streams and have tests that involve pulling or pushing data to them, I'd suggest using `try-put!` and `try-take!` with timeouts in your tests, instead of simply using `put!` and `take!`.
