---
layout: post
title:  "Notes on Youtube Architecture from 2012"
date:   2019-08-16 00:18:23 +0700
categories: [youtube, python, scalability, architecture, til, jitter]
---

### Youtube was built on Python and MySQL and managed to support in 2012 up to 4 billion views / day

Even though the references are pretty old, I was not aware the initial architecture versions of youtube were built on top of python.

If the architecture was able to support up to 4 billion views a day back in 2012, I would be curious how much the architecture changed since then.

So maybe, smaller business shall stop worriend if python can make microservices live up to expectations in terms of NFRs.

#### Tech Stack Notes

The complete stack references can be found [here](http://highscalability.com/youtube-architecture) and [here](http://highscalability.com/blog/2012/3/26/7-years-of-youtube-scalability-lessons-in-30-minutes.html). I will move away from duplicating the information. 

In this page I will mainly cherry pick in the next sessions a few aspects I found interesting to recall.

1. psyco, a dynamic python C compiler

    **Currently psyco is unmaintained and dead. It was replaced by PyPy, as JIT compiler for Python**

    By wiki, just-in-time (JIT) compilation is a way of executing computer code that involves compilation during execution of a program (at run time) rather than before execution.

    This may consist of source code translation but is more commonly bytecode translation to machine code, which is then executed directly. 

    A system implementing a JIT compiler typically continuously analyses the code being executed and identifies parts of the code where the speedup gained from compilation or recompilation would outweigh the overhead of compiling that code.


2. Scalability Strategies

    - Divide and Conquer: partitioning is key, sharding, spreading everything across multiple services, and figure out the communication between the spreaded parts 

    - Aproximate Correctness: if the user cannot perceive the system inconsistency, then the reported state of the system is not inconsistent. Truth being told, the business model fits well with this scheme. As user comments consistency can be tricked, in comparison to banking transactions.

    - Export knob twiddling: ex using different consistency models based on different parts of the system, ex: renting a movie once a payment is being made, shall be consistent. 

    - [Jitter](https://en.wikipedia.org/wiki/Jitter)

    - Cheat: know how to fake data

    - Local Optimizations

    - [RPC](https://www.tutorialspoint.com/remote-procedure-call-rpc)

    - Well defined **inputs** and **dependencies**

    - Have **good data / schema definitions**

    - Don't use dict as schema contract which can have anything

3. Efficiency has to be traded for Scalability

    The most efficient execution would be havving everything crambled together. But that would not be very scalable. Therefore it is always a tradeoff to figure out how to break out the components.

    - learn to measure

    - focus on the algorithms

    - efficient libraries in python: 
    
    
4. Caching
        
    - Fully formed Python objects are cached. Some data are calculated and sent to each application so the values are cached in local memory. This is an underused strategy. The fastest cache is in your application server and it doesn't take much time to send precalculated data to all your servers. Just have an agent that watches for changes, precalculates, and sends

    - In case of long tailing caching won't always be your performance savior.

5. Serialization will be the most expensive, no matter what library you use. Therefore, measuring becomes critical. Recent podcasts go on about protobuff and RPC.

6. [Vitess](https://github.com/vitessio/vitess)

    Vitess is a database clustering system for horizontal scaling of MySQL through generalized sharding.

    By encapsulating shard-routing logic, Vitess allows application code and database queries to remain agnostic to the distribution of data onto multiple shards. With Vitess, you can even split and merge shards as your needs grow, with an atomic cutover step that takes only a few seconds.

    Vitess has been a core component of YouTube's database infrastructure since 2011, and has grown to encompass tens of thousands of MySQL nodes.

    [Vitess](https://vitess.io/) is a cloud native computing foundation graduated project.

7. Zookeeper - a distributed lock server. It’s used for configuration 

8. Wiseguy - a CGI servlet container

9. Spitfire - a templating system. It has an abstract syntax tree that let’s them do transformations to make things go faster

10. [Apache HTTP Server](https://apache.org/) - Youtube is not async, everything is blocking. Every request goes to Apache

11. [pycurl](http://pycurl.io/)
 

#### More on Jitter

Jitter strategy comes in handy to avoid the thundering herds effects. 

Jitter is used to introduce randomness on things which tend to stack up. Ex: cache expirations - the most popular video on youtube might be cached for 24 hours. If everything expires at one time then every machine will calculate the expiration at the same time. This creates a thundering herd.

By jittering you are saying  randomly expire between 18-30 hours. That prevents things from stacking up. Systems have a tendency to self synchronize as operations line up and try to destroy themselves. You get slow disk system on one machine and everybody is waiting on a request so all of a sudden all these other requests on all these other machines are completely synchronized. This happens when you have many machines and you have many events. Each one actually removes entropy from the system so you have to add some back in. 

As per wiki, in computing, entropy is the randomness collected by an operating system or application for use in cryptography or other uses that require random data. This randomness is often collected from hardware sources (variance in fan noise or HDD), either pre-existing ones such as mouse movements or specially provided randomness generators. A lack of entropy can have a negative impact on performance and security. 



#### Other References

[Scaling Youtube](https://www.youtube.com/watch?v=G-lGCC4KKok&ab_channel=NextDayVideo)

[PyPy Features](https://www.pypy.org/features.html)

[YouTube Strategy: Adding Jitter Isn't A Bug](http://highscalability.com/blog/2012/4/17/youtube-strategy-adding-jitter-isnt-a-bug.html)

[Datatracker IETF](https://datatracker.ietf.org/)

[RPC](https://www.tutorialspoint.com/remote-procedure-call-rpc)

[System Entropy](https://socratic.org/questions/what-is-entropy-of-a-system)

[Paper from 1994 on adding randomization to network trafﬁc sources](http://ee.lbl.gov/papers/sync_94.pdf)





