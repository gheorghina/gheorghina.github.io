---
layout: post
title:  "Handling Backpressure in Elixir"
date:   2020-01-05 00:18:23 +0700
categories: [elixir, erlang, BEAM, backpressure]
---

Backpressure - is the approach of telling the sender to stop ending because there's no room for new messages
load regulations 
    - thoward 3rd party APIs, or service nodes, all you are doing is smoothing out your peaks and throughs, ensuring you don't overflow with requests
    - if you receive requests at a rate faster than you can handle, you will eventualy have to stop queuing and start rejecting

The Little's Law
    The long-term average number of customers in a stable system L is equal to the long-term average effective arrival rate, λ, multiplied by the (Palm‑)average time a customer spends in the system, W; or expressed algebraically: L = λW.
    https://en.wikipedia.org/wiki/Queueing_theory 


    COP - concurrency oriented programming

    having bounded mailbox in Erlang. The general consensus is that this is bad because the mailbox, if unbound via the language, are therefore bound by memory limitations of the computer, and all control is lost (this is no longer true, since OTP 19, added a max_heap_size flag that can be set per process to force an early death if its memory usage is too high, including its mailbox).

    the common overload patterns that can be encountered in Erlang with the regular workarounds available today for your system
 
the overall criticism of flow-control remain true in other concurrent platforms, specifically the preemptive ones such as Go. Cooperative ones (like Akka or node.js) will tend to have implicit mechanisms due to heavy workload blocking the CPU earlier, but may still encounter the same issues depending on workloads.
  
### Conclusion


 


### Other Resources

[Joe Armstrong Thesis](http://erlang.org/download/armstrong_thesis_2003.pdf)

[Designing for Scalability with Erlang/OTP](https://www.amazon.com/Designing-Scalability-Erlang-OTP-Fault-Tolerant-ebook/dp/B01FRIM8OK)

[Handling overload](https://ferd.ca/handling-overload.html)
