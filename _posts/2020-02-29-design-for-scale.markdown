---
layout: post
title:  "Design for Scale with Erlang / OTP"
date:   2020-04-08 00:18:23 +0700
categories: [elixir, scalability, reliability, distribution, DDD]
---


 In these difficult times that we are all living, I was invited to give a talk at the Elixir Berlin April Meetup.
 I am honoured and very happy I had this opportunity. I would like to thank you all who joined and had patience with me.

 Let's do a live one next time!

 I will give here an extract of the ideas I wanted to touch. 

### My background

In my years of career, I can count 4 main projects that shaped the way I think, work and approach things.​
They also offer me a ground for drawing some patterns and being able to share with you today. ​

The first one I would mention, was in Pharmaceutical industry, a backend integration system for aggregating documents and their metadata​. I was involved in reshaping it in such a way that it would allow 100 concurrent search requests with the result time in less than 2s​.

The second one I would like to mention was in the home automation domain where for the first time in my life we managed to reach the max_value in the database for the auto_increment on integer.
 
After 10 years with microsoft solutions, I switched to Elixir – working in a project designed for chat and events streaming – where we had 200k and 300k active connections during peak time​.

Today I work in banking where reliability and fault tolerance are the main focus.



### What is this about?

In this blog post I will try to touch a couple of ideas towards designing a system which leverages OTP for easier to achieve: 
- Concurrency
- Resiliency
- Scalability
- Distribution

 
### The happy scenarios

The happy scenarios in which most probably you don't have to design very complex technical solutions can be any subset of the following:
​
- Stateless APIs which just fetch data​

- No unreliable/less performing dependencies ​

- No need to adapt the workload due to variable load / day  ​​

- The same fix amount of jobs can be always triggered​

- The old trick when talking to the DB is performing good enough for managing the state​

- Enough to just throw more nodes at it 



### Do more nodes always improve the throughput?

The mindset I came close to very often is the one saying: "let's just add more nodes" and it should work.

Unfortunately this is not always the case.

When this topic is raised, I usually tend to check the computational power of each node: ​

- How many cores does the node have?​

- How much CPU does the application use? 
        
- What type of activities are carried on?​

- Is the application taking advantage of the resources efficiently?  

If the answer to the last question is no, then just throwing more nodes at it, will move the needle just a bit.​

​For leveraging CPU efficiently  - I've been down the road of implementing multi threaded applications … and it is hard.  
Thread safety is difficult to achieve, especially in the context of mutating state.​

But this becomes a lot easier in functional programing languages where the state is just transformed. ​

This simplifies the things a lot – making it easier to achieve for example concurrency and be able to take advantage of the multi cores of a machine in using the CPU in a more efficient way.​

​
### 3 million connections on a single node

​The article published by WhatsApp stating they manged to reach 3 million connections on a single node stays in the back of my head. 

This is crazy. I always think about this when we design new features as a reality check, and then try to downscale. 

We tend to overengineer our microservices systems, and we tend to forget that maybe with less we can achieve more.  

3 million connections are difficult to represent, they can be achieved with less than half amount of people, but even so, that is a lot. 


 ![]({{ site.url }}/static/img/3millionpeople.jpg) 



### When the Use Case changes ...

I have seen this happen already to much often, and I am starting to think this is the norm.
We build our applications first, when all that matters is having them work. 
But after a while, the number of expected concurrent requests, the number of events increases, we have to deal with variable load.

I want to put here a wise advice given by a wise man: "Prepare for failure, but also prepare for success". 

What if your application gets traction, when suddenly it may be expected from it to support more load, or perform better.
Iterative design is being encouraged, don't overengineer they say. And I totally agree, but I also agree there is a bare minimum necessary these days, so that when you reach this point, you don't have to redesign your application completely. I've seen this happen already 3 times.



### First steps …

Therefore, no reason to panic. Measure everything you have under the expected load everything. Test your system resiliency.
Adjust only after based on the concrete extracted numbers​, and reiterate. Reiterate so many times until you are happy with the measurement numbers you get. 



### Throughput vs Scalability

Response time: This is the most widely used metric of performance and it is simply a direct measure of how long it takes to process a request.​

​Throughput: A straightforward count of the number of requests that the application can process within a defined time interval. For Web applications, a count of page impressions or requests per second is often used as a measure of throughput.​

​System availability: Usually expressed as a percentage of application running time minus the time the application can't be accessed by users. This is an indispensable metric, because both response time nor throughput are zero when the system is unavailable.​​

A perfectly scalable system is one that has a fixed marginal cost to add additional users or capacity.​

Scalability is closely related to maintainability. We usually develop very fast the applications and we spend the rest of the time maintaining them.

Scalability is a characteristic of a system, model, or function that describes its capability to cope and perform well under an increased or expanding workload or scope.  ​

A system that scales well will be able to maintain or even increase its level of performance or efficiency even as it is tested by larger and larger operational demands.​



### The Importance of Supporting Scalability 

As mentioned above, as the business grows, we need it for being able to meet market demands which always shift based on how people interests and tastes change.
The scalability capability of your system can make you stay competitive in these circumstances. Can make your system being able to cope with increasing demands of working with more customers, data and resources while also reducing your costs. 



### The Mind Shift with Erlang / OTP

    "We do not have ONE web-server handling 2 millions sessions. ​

     We have 2 million webservers handling one session each"​
                                                        Joe Armstrong​

This mind shift is the key.  It is not that easy when switching from different languages to actually perceive these concepts and suddenly start building concurrent fault tolerant applications​. 
It is a lot easier in the beginning to be trapped into trying to rebuild the concepts you are already familiar with from  .NET, Java or Ruby.

But I say this is key – the game changer with elixir/erlang applications.

​If there is a software error and the server software crashes we lose either two million connections or one depending on the model.​
The most important part is that one session crashing shall not effect all the other sessions.​ 

  
### Design to Scale with Erlang / OTP

 - Concurrency with Processes ​

    Erlang's main strength is support for concurrency. 

    Erlang has a small but powerful set of primitives to create processes and communicate among them. ​

    These processes are the primary means to structure an Erlang application. They are neither operating system processes nor threads, but lightweight processes both in terms of memory and CPU utilization that are scheduled by BEAM.  

    They share no state with each other, thus many can be created without degrading performance. Tens of thousands of processes can be executed simultaneously on a single node.

 - Message Passing  ​

    Though all concurrency is explicit in Erlang, processes communicate using message passing instead of shared variables, which removes the need for explicit locks, a locking scheme is still used internally by the VM).

 - RPC / Distributed Tasks  ​

    These processes facilitate building distributed applications.​
    
    RPC and Distributed tasks are built-in Erlang/Elixir abstractions that allow communication using Elixir term without any additional serialization and deserialization. 
    
    The common approach for communication with other applications which are not written in Elixir is using HTTP protocol. 

 - Supervision
    
    A typical Erlang application is written in the form of a supervisor tree. 

    This architecture is based on a hierarchy of processes in which the top level process is known as a "supervisor". The supervisor then spawns multiple child processes that act either as workers or more, lower level supervisors. Such hierarchies can exist to arbitrary depths and have proven to provide a highly scalable and fault-tolerant environment within which application functionality can be implemented.​

    Within a supervisor tree, all supervisor processes are responsible for managing the lifecycle of their child processes, and this includes handling situations in which those child processes crash. Any process can become a supervisor by first spawning a child process, then calling erlang:monitor/2 on that process. If the monitored process then crashes, the supervisor will receive a message containing a tuple whose first member is the atom 'DOWN'. The supervisor is responsible firstly for listening for such messages and secondly, for taking the appropriate action to correct the error condition.​

    In addition, "Let it Crash" results in a style of coding that contains little defensive code, resulting in smaller applications.​

### Leverage OTP for Scalability​, Some Good Practices Advices 

The profile of the application will dictate the way the application will scale. Therefore you have to understand your boundaries, your data flow and the load types. 
The following are just generic practices, that have to be adjusted to each specific case.

 - Dynamic Named Processes ​

    To be able to spawn them dynamically with an increasing dynamic load, and named for being able to find them. 

 - To Cluster or Not To Cluster​

    Will be dictated again by the application. The easiest case is when processes can be created to work independently to each other.
    
    The more complex case is when nodes have to be connected to each other an communicate through messages.
    
    Most of the times, for being able to take advantage of each node's capabilities, machines tunning might also be required.

 - Group Processes on the same node​

    This will help in building more reliable, performant systems. Otherwise the network transfer can be a bottleneck.

    The processes work is dictated by the set of Schedulers which is tightly related to the number of the cores. 

    The more cores the nodes have, the more concurrent processes can process data​.

 - Supervision trees 

 - Define Your Communication Channels​

 - Caching

    External vs Internal caching strategies can both work, depending on your case.

    External caching systems will help in not having to care about system failures / maintenance windows or redeployments. 

    You will just have to come up with strategies for expiring your keys.

    On the other hand, internal caching can give you power into being able to stay up and allow some core functionalities, in times of 3rd parties or other dependencies failures. 

 - Persistence as backup for recovery​

 - Idempotent actions​

    ![]({{ site.url }}/static/img/there_are_only_2_problems_in_distributed_systems.png) 

    Depending on how your communication channels are defined, in distributed systems, the easier to implement is "At Least Once Delivery". 
    For example, if you use an external messaging system, you shall be prepared to receive the same message multiple times.


 - Exponential backoff retries​

    It may be that your application behaves well in times of horror. But some dependency other one not. 
    Therefore, proceeding with aggressive retries can make things worse.

 - Resiliency to dependencies failures​

 - Use Poolboy​  

 - Throttling​

 - Planning for Overload – Back-pressure & Load-shedding

    Have many processes that act as buffers and load-balance through them (scale horizontally).

    Use ETS tables as locks and counters to reduce the input.​

 

### Measure everything... so that you can adapt

 One of the best selling points of the Erlang VM for production use is how transparent it can be for all kinds of introspection, debugging, profiling, and analysis at run time.
​
 - Number of Processes​

 - Total Messages ​

 - Run Queue Length​

 - Port Count​

 - GC Count​

 - Reduction Count​

 - And most importantly, your own custom metrics


All the above metrics can give you eyes on how your system performs, if all goes well or if there are adjustments required.
I advise all of you to check them out. 

The total messages can give you insights into how well the processes behave, they can help you discover bottlenecks at the process level where the sequentiality happens​. 

​
### Fallacies

These list displayed bellow applies to any other system, independent of the chosen technologies or languages.
These are the small things we usually forget, and later on bite us hard. I am listing them here mainly for me so that I will not forget.

 - Don't trust the network​

 - Don't trust the dependencies / 3rd parties​

 - Don't trust your surrounding infrastructure​

 - Don't think there is no latency or that the bandwidth is unlimited​

 - Don't think the topology cannot change​

 - Don't be afraid to add tons of Logs
​

### It is always a trade off ...

    A scalable, reliable system will not necessarily be performant.

This is the dance you have to play. Your stakeholders might not be all technical and be able to comprehend all the above concepts in detail.
Therefore it is your responsibility in presenting the tradeoffs in a transparent manner. 

You might have a very scalabale, performant system but it might loose some of its reliability. 
If on the other hand you need to deal with tons of load in a very reliable way, then the performance might be lacking.

We have to make compromises, which may vary based on what can be acceptable or not... make sure you set the expectations right. 


### Conclusion

 If you are in Berlin, please join the club by just paying a visit or also sharing your experience with us. 
 The more the merrier for opening the door to knowledge sharing.


### Resources

[Elixir Berlin April Meetup](https://www.youtube.com/watch?v=19XPpsKLG_E) 

[Performance vs Scalability](https://blog.professorbeekums.com/performance-vs-scalability/)

[Managing Two Million Webservers](https://joearms.github.io/published/2016-03-13-Managing-two-million-webservers.html)

[Erlang Docs](https://www.erlang.org/docs)

[OTP Design Principles User's Guide](http://erlang.org/doc/design_principles/users_guide.html)

[Learn you some erlang](https://learnyousomeerlang.com/content)

[Designing for Scalability with Erlang/OTP: Implementing Robust, Fault-Tolerant Systems by Francesco Cesarini​](https://www.goodreads.com/book/show/18324312-designing-for-scalability-with-erlang-otp?ac=1&from_search=true&qid=P4wrIH1fMr&rank=1)