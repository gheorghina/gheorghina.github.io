---
layout: post
title:  "Transitioning from C# to Elixir"
date:   2019-12-30 00:18:23 +0700
categories: [elixir, c#]
---

This article is dedicated to all my coder fellows who ask me what is that magic potion I am boiling these days, and to all those out there being afraid to try the leap of transitioning from their language of choice to Elixir, by thinking they will become less relevant on the workforce market.
 
Beware my friends, next time you ask me about Elixir, Absinthe or any other magic cocktail I will just provide a link.
 
So this is my journey on transitioning from C# to Elixir.
But if I think deeply, there is no such thing as transitioning completely from one language to another. It is more about expanding your knowledge as a developer, which I encourage everyone to do, and then be able to use one or the other as they are a better fit.
 
Some people advise on learning one new language every year, but this might be a bit extreme if you are like me wanting to know the insights well enough to be able to provide solutions fit for the business.It can take up to several weeks to learn a new language, but this in my opinion implies only knowing how to code in it.
 
If you want to go further, deeper into the rabbit hole, then for sure there are more factors to it. From knowing how to code in a language and be able to design features means making one leap further to understanding different solution variations with their pros and cons.
For this, you need the context, the people, the books, the time, the patience and the brain.
 
After 10 years of providing Microsoft Solutions for Enterprise Businesses, I needed a change. 
At that time I did not know exactly what Elixir or Erlang is, what power do they bring, what difference do they make. I just had this huge opportunity, that through a thorough interview process I was accepted in a team. They worked on a project written in Elixir which was used for Chat and Events Streaming between multiple systems.
 
When I decided to accept the job, my eagerness started to grow, but also my anxiety. Just so you know, in Cluj Napoca(Romania), where I am from, there are only two companies working with Elixir at the time of writing this article, out of which only one has a complex, heavily utilized production system. 
So it was not like I had many options where to go and ask. 

What do you do in this situation? 

Well, you just start digging the internet, do tons of research for gathering insights and learning materials and try to see if other people went through the same process or if you are all alone. 

I added at the end of this article a link to another post I made with resources which helped me a lot during the process. Some of them I am still using today and will continue to use as long as I will work with the Erlang VM languages.

Therefore, in the next lines I will not write about which of the two is better, since both serve, very well, different purposes. 

I will go through some of the key points which sold Elixir to me. 
I will present through my subjective eyes, things that made a difference for me in how I approach things.    


### What is it?

[Elixir](https://en.wikipedia.org/wiki/Elixir_(programming_language)) is a dynamic, functional programming language created by José Valim. It is built on top of [Erlang Virtual Machine called BEAM](https://en.wikipedia.org/wiki/Erlang_(programming_language)). 
It is used when either one of the following are required: 

1. Robustness and fault-tolerance 

2. Concurrency

3. Scalability

4. Distribution 

5. Real-time communication

6. Heavy traffic   

 
The way I viewed it in the beginning, was that Elixir was developed for programmers to be able to quickly grasp it and implement powerful systems on top of BEAM which otherwise would not have been achieved since Erlang falls at the bottom in the popularity charts. 

What also helped me back then was knowing there are well known applications which are being built with Elixir. 
I will name a few: Whatsapp, Pinterest, Discord, Postmates, Bleacher Report, PagerDuty and [many others](https://www.erlang-solutions.com/blog/which-companies-are-using-elixir-and-why-mytopdogstatus.html). 
 
 
### What was it like to transition? 


- The learning curve:

    I would say it can take up to one month to get comfortable writing Elixir code. 
    You will not know all the insights but due to its simplicity and clean syntax, you will be already able to add functionalities to an existing code base.

    If you want to get comfortable providing more complex solutions, based on the language capabilities, it would take a couple of more months during which I would recommend to take you time to read as much as you can, watch presentations and try new things.   

- Working with dynamic languages

    One of the biggest hurdles with Elixir is that it is a dynamic language. You have to leave the cosiness of static typing you have in C# and keep your brain more active, since the IDEs are not that developed at this stage. 
    The code reading experience becomes more challenging. Therefore, simplicity and good data types become critical when writing elixir code.
    
    [Dialyxir](https://github.com/jeremyjh/dialyxir) is currently used for static code analysis and also for having a quicker glimpse on the function definitions( what input they expect and what output they return) 

- Reliable distributed systems in the presence of software errors
 
    For the ability to implement a self healing system, which supervises and fixes itself, which is continuously running with very low to none maintenance a revolutionary way of thinking was required. And it was: [Joe Armstrong's thesis](http://erlang.org/download/armstrong_thesis_2003.pdf)

    Elixir runs on Erlang OTP [actor model](https://www.brianstorti.com/the-actor-model/) and takes advantage of every aspect of the OTP.

 - The immutability nature of Elixir

    While working with OOP, the need for knowing and applying design patterns, knowing anti-patterns becomes a must, a religion. 
    This is not necessarily bad. I understood over the years the value added by them: you need some means to manage chaos. And those means need to be recognizable by the vast majority of people.
    
    The complexity is induced by: 
     1. variables which can be mutated, making programmers spend their most time investigating the values which are being unexpectedly changed
     2. classes which have to be instantiated, making you end up with a pool of instances which have to be passes correctly around
     3. too much inheritance, so therefore people are told to favour composition 


    So you have no choice but end up with the must of knowing and working with Inversion of Control Containers, Dependency Inversion Principle, Factory Pattern, Singleton Pattern, Strategy Pattern and so on. 

    Without order there is chaos -- therefore GOF. But I will not praise it more. 
    There are all sorts of malformations and weird solutions to which instead of fixing and cleaning the code, we are applying patterns with no remorse.  
    A very good example which falls in this category is the [Visitor Pattern](https://www.oodesign.com/visitor-pattern.html)  


    In C# with all the above it can become really though to build concurrent, scalable systems which also manage state. I am not saying it is impossible, I am just saying it is hard. 
    The ease with which this can be accomplished in Elixir which runs on top of Erlang OTP model makes it a breeze. 

    This might be one of the reasons people who switch to BEAM languages prefer to stay. 


 - The community and the BEAM ecosystem 

   The Elixir community is very supportive. They are doing visible efforts in helping newcomers, you just have to go there and ask.
   
   What I also like is that even if there are gaps in the libraries, you don't find tons of libraries which do the same thing. Making your life a lot easier when choosing the tools suite for doing the job.  

   Maybe, but just maybe, one of the missing tools I would really need also for Elixir solutions is the [Performance and Load Tests Project](https://docs.microsoft.com/en-us/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2019) which allows you to define very complex load and performance tests for your APIs. The list metrics provided at the end are very detailed and helpful. 

  
### Conclusion


With this blog post, I just wanted to let others know they are not alone. 
There are others like you switching to Elixir. Have no fear, be eager and open to learn new things. Be curious and then good things will happen to you. 

It is true that there are fewer job opportunities, the community is not that big. There are missing tools. But this just leaves room for making an impact, by bringing value in a continuously growing ecosystem.

Besides, there are only advantages to knowing, understanding more languages. 
It will make you a better craftsman who will have less chances of falling under the ["Hammer law"](https://en.wikipedia.org/wiki/Law_of_the_instrument) 


### Some Resources


[Elixir resources I put together](https://gheorghina.github.io/elixir/erlang/resources/2019/01/31/elixir-erlang-resources.html)


[C# vs Elixir discussion on the Elixir forum](https://elixirforum.com/t/elixir-vs-c/670/13) 


[Check what other people listed as pros and cons on both](https://www.slant.co/versus/115/1540/~c_vs_elixir) and maybe [this one](http://vschart.com/compare/elixir/vs/c-sharp)


[Ease the fear of losing your relevance](http://devonestes.com/the-truth-about-hiring) and [this one for job openings.](https://functional.works-hub.com/jobs/)  --- but if you start searching you will see there are more and more companies realising the value the BEAM VM brings and are slowly starting to migrate their solutions.

