---
layout: post
title:  "The curious case of a Recurrent CPU Spike"
date:   2021-02-01 00:18:23 +0700
categories: [elixir, cpu spikes, aws, remote connection, epmd]
---

Truth being said, I am not sure event to this day how to investigate properly CPU Spikes in Elixir.

Various tools, which have $top capabilities offered no support in identifying root cause for the bellow case.

### The Inception

When CPU related alarms call suddenly for CPU utilization hitting 100% suddenly with no means of completing its activity, all one can do is start digging around to check what external or internal activity can cause it.

When no increase spike in external calls happen, when no abnormal obvious internal activity is visible and when no SQL query is being slow, one can loose hope in finding root cause to such behaviors.  

What worked in this case was time, tons of patience, rabbit eyes and even a bit of paranoia during each incident in trying to correlate the CPU spike to any activity.

 ![]({{ site.url }}/static/img/high_cpu_spike.png) 


### The Investigation Approaches

WIP

### Resources

[Elixir Forum Topic](https://elixirforum.com/t/expired-ssm-agent-session-where-a-debug-node-is-connected-via-iex-remsh-does-not-disconnect-automatically-the-node-leaving-nodes-state-inconsistent-bonus-cpu-on-service-node-spikes-instantly-to-100/38990) 