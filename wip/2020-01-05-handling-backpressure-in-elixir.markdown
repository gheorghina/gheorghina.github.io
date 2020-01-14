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

blocking processes...cascading the issues to all processes, making the entire system fail

the idea of making the boundaries with 3rd parties systems async

In such a system, if your server is able to handle 500 concurrent requests and then it stops accepting more, that's how much load you can plan for. You get an easy upper limit. Few servers, whether they be web servers, socket servers, or whatever kind, will tend to impose a limit, even though it can be done: we all like the idea that we can scale linearly and are not happy with the idea of a known low-level of concurrency being in place for the sake of stability. Indeed, it'd be way nicer to get our stability through less drastic means.

The case of a perfectly parallelisable system is fairly trivial to handle, still: benchmark until you hit a breaking point, put a hard limit on concurrency (make the whole server a pool), and then scale horizontally by adding more servers. This case is not very interesting to handle system-wide because it is fairly rare, but it is still going to be valuable for subcomponents of the system or as part of some optimization (concurrent connection pools come to mind here).

How to handle this in Erlang
Most pooling libraries offer a simple way of doing this, and many offer fancier control than what would otherwise be available:

poolboy
pooler
leo_pod
gproc
varpool
Note that I am omitting pools more adapted to specific tasks, such as those dedicated to network clients or database connection handling. 

The naive backpressure
In the case of Erlang, asynchronous is the default as soon as we introduce multi-process communication. Given the more complex diagram from earlier:



Process 3 is going to be at a risk of getting an overflowing mailbox if other processes on the node send too many messages its way.

The easy way to fix this is with backpressure, by making all calls to process 3 synchronous. This means that whenever processes 1, 2, or 4 send a message to process 3, they won't be able to send another one until process 3 has responded to one of theirs because every one of them will be stuck waiting for a go-ahead signal to send more.

Instantly, this limits the amount of concurrent work possible to do and prevents overflow by slowing the system down.

Backpressure
Mechanism by which you can resist to a given input, usually by blocking.

Load-shedding
Mechanism by which you drop tasks on the floor instead of handling them.
  
### Conclusion


 


### Other Resources

[Joe Armstrong Thesis](http://erlang.org/download/armstrong_thesis_2003.pdf)

[Designing for Scalability with Erlang/OTP](https://www.amazon.com/Designing-Scalability-Erlang-OTP-Fault-Tolerant-ebook/dp/B01FRIM8OK)

[Handling overload](https://ferd.ca/handling-overload.html)
