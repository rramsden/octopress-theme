---
layout: post
title: "Appmon not working with rebar"
date: 2012-07-10 21:12
comments: true
categories: rebar, erlang
---

I recently setup my development environment using rebar. However, my two favorite debugging
modules weren't getting bundled into my development release `appmon:start()` and `tv:start()` were
returning undefined. If you want to enable tv and appmon you will need to bundle them into 
your rebar release. You can do this by editing your reltool.config in rebar. Like so: 

    {rel, "appname", "1",
    [
      application1,
      application2,
      ....
      ....
      {tv, load}
      {appmon, load}
    ]}

Build your release and appmon and tv should be available :)
