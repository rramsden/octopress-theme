---
layout: post
title: "Gossiping in Distributed Systems - Part IV"
date: 2012-07-02 20:39
comments: true
categories: gossip protocols, gossiping, cloud computing
---

In my [last post](/blog/2012/06/27/gossiping-in-distributed-systems-part-ii/) we went over a very simple
Gossip-based algorithm called the Symmetric Push-Sum Protocol or SPSP for short. What I didn't mention in my last post was the fact
that there are three types of Gossip protocols. SPSP falls under the [aggregation](http://en.wikipedia.org/wiki/Gossip_protocol#Gossip_protocol_types) category.

This time round I'd like to cover [Anti-entropy](http://en.wikipedia.org/wiki/Gossip_protocol#Gossip_protocol_types) protocols.
These protocols are designed for repairing replica data. This is the type of gossiping you see going on in distributed data stores like Cassandra, Riak, 
Amazon's Dynamo, etc. They come in handy when we need to synchronize some state between nodes.

Synchronizing State
-------------------

Assume that each node has a hash table of of key-value pairs. Now imagine each value has its own version number.
Once gossiping begins nodes will exchange state information and reconcile their differences by comparing version numbers.
This reconcilation operation can be done a number of ways. Were simply comparing two hash tables and relpacing
outdated version of data. Will use Merkle Tree for this.

Three Types of Gossip Reconciliation
------------------------------------

**Going to pick this up in the morning**

