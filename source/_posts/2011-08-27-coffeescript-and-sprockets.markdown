---
layout: post
title: "CoffeeScript and Sprockets 2.0"
date: 2011-08-27 14:58
comments: true
categories: [CoffeeScript, Ruby, Rails]
---

Sprockets makes it really damn easy in a few lines of ruby to concatenate and serve your coffee script files. 
It supports multiple template engines ERB, JavaScript, CSS, SCSS, etc you can 
even add a dash of ERB <%= %> to your coffeescript source files by naming your file source.coffee.erb and sprockets will automagically compile it 
down with ERB then CoffeeScript and concatenate  it with other dependencies!

For the project I was working on I couldn't depend on Node.js for dependency management since I was creating a front-end browser game. At first I decided to implement my own hacked up solution in Ruby to substitute and concatenate coffee script source files... 
it wasn't pretty :( After moving to sprockets I was able to refactor a few hundred lines into three :)
For example, suppose you have a file that contains sprockets `require` syntax called Main.coffee

{% codeblock lang:ruby %}
#= require "src/one"
#= require "src/two"
#= require "src/three"
MyApp ?= {}
{% endcodeblock %}

Then all you need to do is create a Sprockets::Environment instance and add the source path

{% codeblock lang:ruby %}
require 'sprockets'
env = Sprockets::Environment.new
env.paths << "src/coffee"
open("example.js", "w").write( env["Main.coffee"] )
{% endcodeblock %}

Tada! It just works :)
