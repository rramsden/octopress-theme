---
layout: post
title: "Gossiping in Distributed Systems - Part I"
date: 2012-06-25 17:36
comments: true
categories: 
---

This will be my first write up since I [foolishly committed](http://rramsden.ca/blog/2012/06/23/im-going-to-blog-every-single-day/) 
to blogging every single day for an entire year.  So if your just following along I usually pick a topic I don't know anything about and 
write and try really damn hard to figure it out.  Do I know anything about Gossiping in Distributed Systems? No. Do I ever know
what the hell I'm doing? No. But I'm here and I'll give it my best shot ;) That being said if you haven't closed your browser 
yet I'm going to break this post up into a few blog entries since I probably can't become an expert on gossip protocols over night.

Where do we start?
------------------

Lets start with explaining Gossip communication and what it means in distributed systems.
For example, perhaps a co-worker spreads a nasty rumour about you. Apparently you 
haven't been testing your code (shame on you). Your colleagues A and B chit chat over lunch and A
mentions your bad habit. Once A and B interact with more people the rumour spreads exponentially faster.
Pretty soon the whole office is glaring at you and you become the scape goat for every single problem
in the office. Eventually this rumour mutates and your boss hears your purposely unfactoring your code and using
pig latin for your method definitions. You end up being fired :(

Similarily, in a distributed system its easy to tell one node a message and have that message
quickly propogate out to a few servers, maybe thousands with amazing speed.
Wikipedia provides an excellent [example](http://en.wikipedia.org/wiki/Gossip_protocol#Examples) of a data center 
that houses 25,000 servers. A search term is passed to a central point where it is propogated out. Since
one of the machines has the item being searched for its able to receive and send a result back within 3 seconds in the
worst case scenario.

Next Page - [The Push-Sum Protocol](/blog/2012/06/27/gossiping-in-distributed-systems-part-ii/)
