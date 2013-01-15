---
layout: post
title: "Producer-consumer problem in C"
date: 2011-08-27 14:10
comments: true
categories: Algorithms
---

For my Operating Systems class CSCI360 we had to write the classic Producer-consumer problem in C using semaphores. 
This is a classic problem/solution to preventing race conditions over a shared resource.

For those of you who don't know what a semaphore is it's simply a variable that acts as a counter ( with two operations up/down ) 
which puts a process to sleep if it tries to down/decrement a 0 valued semaphore. 
The process will receive a wakeup signal once the 0 valued semaphore is up/incremented by another process.

In the producer-consumer example below we have a critical region (some shared buffer) that needs to be 
controlled using semaphores to prevent race conditions. 
We create two semaphores one called EMPTY which represents the number of empty slots available 
for the producer and another semaphore called FULL to represent the number of slots occupied with items.

If there are no items in our buffer a consumer will goto sleep since it will try to down the FULL counter which will 
be 0 because there are no items occupying any slots for it to consume. 
Once the producer creates an item it will down EMPTY and up FULL; vice-versa. 
In the case of the consumer when it consumes items it will up EMPTY and down FULL. 
This might seem a little confusing at first so I created a diagram to 
illustrate the process (note: producer/consumer are NOT running in parallel in this diagram).

{% img http://img.skitch.com/20110827-q7hd4p6n938dfnix18arm2sjwa.jpg %}

It is important to note that EMPTY and FULL alone do not prevent race conditions.  
This doesn't cover the case when the buffer is neither full nor empty. 
This means we need a third semaphore to lock the shared buffer to prevent consumers and producers from accessing 
the shared buffer at the same time. 

We call a semaphore with a single counter (initialized to 1) 
a binary semaphore or more commonly referred to as a mutex or lock. Using a mutex initialized to 
1 when a consumer wishes to consume an item it simply needs to down the mutex before consuming 
the item which will prevent other producers/consumers from manipulating the shared buffer.
There are two main semaphore implementations in UNIX-based systems: System V (available in older-systems) and POSIX. 
For my example program I used System V implementation.

Producer.c
----------

{% gist 1175897 %}

Consumer.c
----------

{% gist 1175900 %}

Shared.h
--------

{% gist 1175904 %}

Shared.c
--------

{% gist 1175903 %}
