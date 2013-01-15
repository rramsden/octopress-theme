---
layout: post
title: "Teensy 2.0 USB Problems"
date: 2011-10-23 16:06
comments: true
categories: microcontroller,arduino,teensy
---

A few problems I came across playing around with my new Teensy development board. Decided to
make a quick post on some solutions I found. Enjoy :)

Problem #1
----------

Teensy USB device wasn't being recognized. You can see the list of
registred devices on Ubuntu using `lsusb`

Solution 
--------

Loaded program code may interfere with the USB registration process.
Use the RESET button then check the output of `lsusb` again make sure the device
is being recgonized. NOTE: you may have to hold it down and plug the USB device in then release it.

Problem #2
----------

If that wasn't enough my USB device was being registered under `/dev/hidraw0` which
isn't recognized by the Teensy Loader Application. It only looks at /dev/ttyACM0

Solution
--------

When the kernel registers the USB device it doesn’t give regular users write/read
access to the device. You will need to setup appropriate udev rules to do this.
udev rules are available from the Teensy website http://www.pjrc.com/teensy/49-teensy.rules 
you will need to download this file and place it in /etc/udev/rules.d/
