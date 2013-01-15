---
layout: post
title: "There and back again"
date: 2012-06-29 22:40
comments: true
categories: erlang, ruby
---

When my boss asked me if I wanted to take a break from the Ruby on Rails world and 
work with him on our Erlang backend I was pretty stoked. Here was a chance to try out a new
language, a functional one, something different than anything I've encountered before.
So that day I decided to put down my web developer hat for a bit and that week started cramming everything Erlang.

My first impression of Erlang wasn't all that good. Looking at its syntax made me cringe a bit. Coming from 
a background in Ruby I was use to reading code as if it was written in plain old english.
This wasn't the case with Erlang. Staring at my first bit of Erlang code I had no clue what the heck was going on.
However, I pushed on. About a week in I started familiarizing myself with the syntax. And you know what? 
Despite what everyone says I think the syntax is just fine. Try spending a bit of time learning Erlang
and you'll realize it isn't half-bad.

That being said, the syntax in Erlang wasn't the thing bothering me. It was because working in a functional language
meant I would have to change the way I think.

To understand recursion, you must understand recursion
------------------------------------------------------

I remember a conversation I had with one of my Computer Science professors about recursion. She told me
that although recursion is totally awesome its usually frowned upon when you try and sneak it into production code.
She told me about her experience at IBM during her internship. In short, her co-workers weren't too happy about her *creative* solutions.

Working in a functional language means you can't avoid recursion. Its everywhere. Functional languages
impose a share-nothing architecture. This means everything is immutable. In order to modify something you would have
to create a new data structure. To prevent your code from looking like crap you would have to use recusive functions to build lists and
other data structures.

Erlang vs Ruby
--------------

The great thing about Ruby is it allows you to use Metaprogramming. The horrible thing
about Ruby is it allows you to use Metaprogramming. What I noticed about Erlang is 
if I wanted to know how something worked I could open up the source code of some library
and take a peek. Within a few minutes I could grasp what the heck was going on. In Ruby, not quite.
You would have to pray the library was documented well enough to tell you what the heck was going on.
There are times when Metaprogramming helps readability however I often see it being misused by
library authors. Ruby looks innocent on the outside but its a complex beast once you start getting
into more advanced topics.

Working with Ruby Again
-----------------------

I started working on hobby projects again in Ruby sometime around April. 
Coming back after almost a year of working with a functional programming language was a bit weird. 

What I love about Erlang was pattern matching. Writing Ruby code I would always have
a sad face when I ran into cases where pattern matching would DRY up code. You've probably run into
situations where you have to bust out the old case statement to switch on arguments passed into a function.
It looks ugly and leads to your eyes having to jump around different logical paths.

I also noticed my way of thinking has changed. The first thing I think about is how
to write something recursively instead of the other way aroud. Would it be fewer lines of code
and more readable if I wrote this recursively? Most of the time the answer was no. When I did find
oppurtunities to use recusion it simplified hundreds of lines of code :) My X11 library for Ruby
constructs packets which contain sub-packets which was a good chance to use some [recursion](https://github.com/rramsden/ruby-x11/blob/master/lib/X11/form.rb).
Gotta love those moments when you can use recursion to simplify things.

Anyway, I've ranted enough. In retrospect becoming proficient in a functional programming language
has improved my imperative programming style immensly.



