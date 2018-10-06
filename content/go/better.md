+++
date = "2018-09-16T09:51:54.008Z"
draft = "true"
slug = "better"
tags = ["go", "golang", "concurrency", "channels", "goroutines", "aws", "python", "python-flask", "python-celery", "python-flower"]
title = "Replacing Python with Go"
abstract = "This is a case study of how we were able to replace Python (Flask, Celery, Flower) with Go at AF Radio, a radio startup in Accra, Ghana. While we hypothesised on the benefits, post-deployment confirmed them. The immediately obvious benefits were: straightforward concurrency, less infrastructure, cost-effective distribution, and a single binary."
+++

With [Bubunyo][BubuTwitter].

## Motivation

[AF Radio][AFRadio]\* is a startup serving the
world of radio lovers out of our base in Accra,
Ghana. Our goal, put simply, is to improve the way
radio lovers consume radio and its contents. One
of the services we provide to both radio stations
trying to go online and their listeners is **Radio
Playback**, which makes shows available for
inexpensive streaming after they originally aired.

This post talks about how Go, its standard
library, and a Go SQLite3 driver were enough to
build the current playback recording and
distribution system.

## The Proof of Concept (PoC)

The first implementation of the recorder had 3
parts: a web server to receive recording requests,
a recording server that saved the stream to file,
and distributor that saved the files to AWS S3 as
well as made them accessible at an API endpoint.

The ease and availability of Python is appealing.
Talk less of the amazing community that has given
us packages that either augment Python's standard
library or elegantly handles the boilerplate in
order to provide a much cleaner and easy to use
API.

## Several Months Into the PoC

Maybe we shouldn't have let the PoC live on for
that long. Or maybe we should have addressed the
technical shortcomings of the PoC with more
resources. Basically, pay more money to AWS.

## Go To The Rescue

## Lessons

Working on the new playback recording system
taught us a few things about programming languages
in general, concurrency and physical limits on
software development in specifics.

### Programming Language Matters

We came to the conclusion that choice of
programming language mattered.

### Concurrency and Parallelism

Because Go made it easy to build concurrent
programs (again, all you need is the `go`
keyword), we overlooked hard limits on the
application such as available RAM and hard disk
space. Investigation into a bug that caused the
recorder to crash every Wednesday at 10AM lead to
the discovery that the recorder was too
concurrent, i.e., parallel to a fault.

So while concurrency made it possible to record
concurrent shows and process the audio files, we
need synchronicity to ensure that all 

_Got comments or corrections for factual errors?
There's a [Hacker News thread for that][HN]._

---
<small>
\* As you probably guessed, at the time of writing,
the **AF** in **AF Radio** didn't stand for **As Fuck**.
</small>

[BubuTwitter]: https://twitter.com/kiddbubu
[AFRadio]:     https://www.afradio.co/
[HN]:          https://news.ycombinator.com
