---
layout: post
title: "Gossiping in Distributed Systems - Part II"
date: 2012-06-27 21:22
comments: true
categories: programming, algorithms, erlang
---

In my [last post](http://rramsden.ca/blog/2012/06/25/gossiping-in-distributed-systems/) we talked
about how how gossiping in distributed systems allows us to quickly propogate messages out to nodes.
In this post will look at and implement a very simple Gossip-based algorithm for computing global aggregates
across nodes in a cluster (ie. sum, average, count).

Σ'ing things up
---------------

Suppose we want to compute a sum across every node in our cluster that gives us the total number of requests/second
our distributed system is handling. Now we could just send messages periodically from each node in the cluster to
some central point. However, our aim should always be for a fullly decentralized system where nodes don't have to rely
on a single machine to calcuate the global estimate.

That being said, were going to use a Gossip-based algorithm called the **Symmetric Push-Sum Protocol** or **SPSP** for short.
Its a simple algorithm for computing aggregates across a cluster.

Symmetric Push-Sum Protocol (SPSP)
----------------------------------
![](/images/blog/spsp.png)

What does SPSP do for us? Well its a 2-in-1 algorithm since we can compute either a sum or an average with it. 
Its also *damn simple* which is why its a good place to start when talking about Gossiping algorithms.

SPSP works by keeping track of a weight and a value at each node. In our case, 
the value would be our request/second service rate at a node. And the weight acts like a percentage telling us
how much of the overall sum we have. Different weight values are defined in the SPSP [whitepaper](http://www.thinkmind.org/download.php?articleid=ap2ps_2011_2_10_30063).
For example, if you wanted to calculate a sum across a cluster you would initialize the weight to be 1 at one node and 0 at the rest. I'll explain
why this is in a moment.

In the example below will pretend that nodes A and B are seeing 5 and 10 requests/second. Once our Gossip-based
algorithm is finished we should be able to query either A or B to figure out that the system is seeing in total 15 requests/second.

![](/images/blog/ex01.png)

Once the system has initialzied nodes can start gossiping. 

![](/images/blog/ex02.png)

Above I've drawn a little sequence diagram showing SPSP in action:

1. Node A will push half its weight and value to Node B
2. Node B receives a push message from Node A. Node B will take half its value and weight and send a symmetric push back to the caller Node A
3. Once Node A receives a symmetric push from Node B it simply adds the received value and weight to its own
4. Finally Node B will add Node A's original push message to its own values

Note: this is all completely asynchronous. Theres no need to block between a push and a symmetric push

When is it finished?!
---------------------

You know when the algorithm is finished when the weight values at each node converge to `1 / N` where `N` is the number of nodes.
Once you reach convergence you're ready to calculate your sum. This part is easy! Take your
value and divide by the weight. In the example above it would be 7.5 / 0.5 = 15

Next Page - [Erlang Implementation](http://rramsden.ca/blog/2012/06/28/gossiping-in-distributed-systems-part-iii/)
