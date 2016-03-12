---
layout: post
section-type: post
title:  Pairing Tour Day 10
category: learning
tags:
- learning
---
I spent the final day of my pairing tour working with James. We worked on implementing the settings page of an Android application.

I've never worked with Android before, and James had just started on the project that week. The application also depends on a SDK for interfacing with a piece of proprietary technology developed by the client.

In the face of these unknowns, James and I made relatively slow progress despite the relative simplicity of our task.

Our primary focus was on displaying the version of the app, the firmware, and the bootloader on the page.

The primary difficulty we faced was testing that these values were correctly retrieved and displayed. There were several steps necessary to correctly render the view, including several object creations and the still-mysterious magic wrought by [Robolectric](http://robolectric.org/).

In the end, we got the job done, albeit with rather more extensive mocking (via [Mockito](http://mockito.org/)) than James and I would have liked.

It was instructive to be exposed to the first time to some of the basic elements of the Android ecosystem - activities, fragments, and various UI elements.

In the afternoon, James was sadly occupied by a prolonged client meeting that I really had no place participating in.

Part of the afternoon I spent attending an engaging 8th Light University presentation by Joe Mastey titled "Manage Your Energy, Not Your Time." He shared a number of insights, but probably the greatest takeaway for me was simply a reminder of the benefits of getting enough sleep, regular exercise, and adequate nourishment.

I also spent a bit of time bouncing a couple ideas off of Mike J about ways to increase the likelihood that people will remember to close up the office properly when  they're the last to leave. I posted a couple reminders on the doors exiting our office, but I think that the algorithm described [here](http://workplace.stackexchange.com/questions/42539/how-to-get-employees-to-set-alarm-when-exiting-the-building) might be an ideal way of delegating responsibility for locking up the office.

I was also assigned my challenges - the final set of projects that I need to undertake before my apprenticeship ends and a decision is made whether to accept me as a craftperson. I spent the last few hours of the afternoon doing preliminary research regarding the challenges - I can't say what, since they're super duper secret.
