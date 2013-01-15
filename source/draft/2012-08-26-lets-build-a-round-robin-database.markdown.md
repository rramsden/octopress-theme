---
layout: post
title: "Let's build a Round-Robin Database :D"
date: 2012-08-26 11:31
comments: true
categories: spawnfest2012, graphite, whisper, RRD
---

Having built my own [Round-Robin Database](http://github.com/rramsden/yasa) two months 
ago for [Spawnfest](http://spawnfest.com) I feel the need to write about implementing one
since I hit a few walls designing and building my own. First, there's really no specificaiton
on how to build one. RRDTool and Graphite vary slightly in implementations which will discuss below.
But first, let's cover the basics

Round-Robin Databases
---------------------

Round-Robin databases are used for storing numeric time series data. Since RRD archives stay fixed in size 
users don't need to worry about running out of diskspace as they add more information. How do they stay fixed in size?
The Round-Robin bit in the title means data is inserted into a buffer in sequential fashion until
it hits the end of the array. When this occurs it jumps back to the beginning of the array and starts overwriting old data.
In other words, each table in an RRD is simply a [circular buffer](http://en.wikipedia.org/wiki/Circular_buffer).

![](http://img.skitch.com/20120902-p6bkiat2teux19bpgx64bni85d.png)

These circular buffers are referred to as **retention tables** in RRD speak. Each retention is defined
by a `step:size` where `step` is your interval in seconds when you expect a new data point to arrive. `size` is how
many of these values you want to record. Let's look at a simple RRD schema:

    1:2 2:2 4:2

This defines three rentention tables. When one retention table has accumulated enough data
its values are aggregated into the next table up. The first table has the **highest precesion**
since its sampling data every *second*. The last table has the **lowest precesion**
since it takes one sample every *4 seconds* which is not very *precise* ;) The last table
is actually aggregating data points from the one below it making its precision even worse!

Consolidation
-------------

The reason these databases are able to store so much historical data is the fact that data points are averaged and
stored in lower precision tables.
However, instead of averaging data points a user can control *consolidation* by using their own *consolidation function*. 
If you've used RRDTool or Graphite they let you create RRD archives with either
AVERAGE, MIN, or MAX for the consolidation function. By default most RRD archives will just use the average. Values from higher precision
tables are then consolidated into lower precision tables when enough data points have been gathered.

A Simple Example
----------------

We will use AVERAGE for our consolidation function for determining how values will be aggregated
into lower precision tables. Will start by inserting data at timestamp **120** with a data point value of 1.

                1:2                    2:2                        4:2
    1. [{___, _}, {___, _}], [{___, ___}, {___, ___}], [{___, ____}, {___, ____}]
    2. [{120, 1}, {___, _}], [{120,   1}, {___, ___}], [{120,    1}, {___, ____}]

You're probably wondering why data was inserted into every table even when we don't have enough
information in the successive tables to start consolidating data points. RRDTool and Graphite define something
called the **XFilesFactor**.  This means the percentage of known data points needed to consolidate one data point in the next table up.
In this example its 50% and in RRDTool and Graphite its set to this value by default.
Since table `2:2` has 1/2 points needed it averages the values in the first table. The last table
then looks at the second and and sees its consolidated a data point and thinks it now has 2 seconds out of 4 needed for
it to consolidate one data point. Let's see how the rest of this works:

    3. [{121, 2}, {120, 1}], [{120, 1.5}, {___, ___}], [{120,  1.5}, {___, ____}]
    4. [{122, 3}, {121, 2}], [{122,   3}, {120, 1.5}], [{120, 2.25}, {___, ____}]
    5. [{123, 4}, {122, 3}], [{122, 3.5}, {120, 1.5}], [{120,  2.5}, {___, ____}]
    6. [{124, 5}, {123, 4}], [{124,   5}, {122, 3.5}], [{124,    5}, {120,  2.5}]
    7. [{125, 6}, {124, 5}], [{124, 5.5}, {122, 3.5}], [{124,  5.5}, {120,  2.5}]
    8. [{126, 7}, {125, 6}], [{126,   7}, {124, 5.5}], [{124, 6.25}, {120, 6.25}]
    9. [{127, 8}, {126, 7}], [{126, 7.5}, {124, 5.5}], [{124,  6.5}, {120,  2.5}]

Let's write some tests!
-----------------------

Since we have an idea of the behaviour we want to implement let's write some tests!

  
