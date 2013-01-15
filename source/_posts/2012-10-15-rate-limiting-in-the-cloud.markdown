---
layout: post
title: "Rate Limiting in The Cloud"
date: 2012-10-15 18:23
comments: true
categories: distributed rate limiting
---

One of the biggest technical challenges our team faced this year was
figuring out how to build a distributed rate limiter. Don't know
what distributed rate limiter is? Here's the problem we want to solve:

> Suppose a CDN (Content-Delivery Network) has a cluster of servers which together
> host online content for some customer A. The CDN wants to restrict
> the amount of network bandwidth given to customer A since the customer has
> only paid for a finite amount. However, due to the
> distributed nature of the CDN’s system incoming requests can arrive randomly
> and can be scheduled to any server in the cluster. To ensure customer A’s
> usage-limit is not exceeded the cluster must work together and
> adjust the usage-limit across all servers.

Sounds simple enough right? Your first approach would probably be to keep things
simple. Setup a Redis instance somewhere and ask it "Hey, has
client A exceeded the usage-limit?".  This [approach](http://chris6f.com/rate-limiting-with-redis)
works pretty good and is similar to our old setup. However, since we have
servers running around the world its a little crazy to pause every request
just to check if a limit was exceeded.


So let's say we define a rate limit 300 requests/second. How do we distribute
this over a few servers.

Design Requirements
-------------------

* Must **never** exceed the rate limit
* Fault-tolerant
* Decentralized


you have a bottleneck before servicing a request you have to block and wait
for a reply back from this server...



Before the company I was working for could move to the cloud we had to solve
an interesting problem. I washir
For those

who don't already know I work on adservers. We services thousand

This year I got to tackle a very fun problem. We needed the ability
to rate limit individual clients which were randomly hammering the crap out
of us :) The kicker was the rate limiter would have to be completley decentralized



