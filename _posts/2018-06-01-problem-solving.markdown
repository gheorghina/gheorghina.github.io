---
layout: post
title:  "Solving Business Problems"
date:   2018-06-01 00:18:23 +0700
categories: [.architecture, .analysis and design, systems design, risks management, RUP]
---

Based on the following anonymous quote: 

**"I am not sure about how you do it, but when I have to take a decision, I first analyze it, I evaluate the risk, I take actions for limiting the bad consequences and only after that I screw it up"**

### Context

For a good amount of years I have been exposed to the context of having to prepare technical concepts for new potential products.

That means transforming the whishes of a potential customer into concrete technical actions with a realistic evaluation of complexity, as much as it could be at the moment of evaluation.  

After doing it for a couple of times I decided to document my thinking process and to also do some workshops with my fellow colleagues for knowledge dissemination and teaching purposes.

That being said, I long cycle of workshops took place over 2 years, where I tried to transform a very scary mythical process into some concrete recipe as a guideline.


### Disclaimer

As I worked in both big and small companies, I can say for sure, that the following strategy can be unnecessary and to heavy in some context.
Depending on the degree of the complexity of the problem, the relationships between involved parties, and the required detailed level for the pitch, what I am about to present can be tailored from taking it fully, to just brush through some concepts or skip it all together. 
 
Therefore, please take everything bellow with a grain of salt as it was meant only as being a guideline for a very well defined context which might not apply to you. Use filters to choose anything you might find useful for you.


### Phase 1 - Requirements Gathering

This is the easiest part, right? Anyone can listen wishes patiently and ask some questions, isn't it?

Well, yes, if you know what you are looking for. 

#### Functional Requirements

The easiest and obvious part in requirements gathering phase is understanding the Functional Requirements. They are easy to spot, as stakeholders who are unfamiliar with the development process will come to you with a pouring rain containing details, dreams and ideas. 

Like for example: I am the mayor of the most futuristic city on the planet. I need an intelligent traffic control & monitoring system.

A couple of advices I would have for the functional requirements are the following:

- don't go into solution mode at all at this stage, focus the discussion only on understanding the need

- try not to miss any one - this shows you listen, care and understand the need

- try to simplify/reduce them - try to understand via questions if there is openness to alternatives, or if the requests are fixed and rigid

- try to identify what would be the subset which would bring most value - this would later help you prepare the concepts for the MVP related subset, growing everything from there incrementally 


#### Non-Functional Requirements

The next stage is gathering the non-functional requirements. This part is the most important one, and the most neglected one during this process.

What are they and why are they important? 

As per Wikipedia, they are "requirements which specify criteria that can be used to judge the operation of a system, rather than specific behavior". 

They are important because it is not the same thing to:

- develop a system for 10 requests/h or for 10 million requests/h

- to have to be up and running 24/7 or only 8/5

- to have to process a job once a day or to have to process high volumes of data

I recommend checking documentation for getting a better grasp on what they are. A good list of quality attributes of a system can be found as well on wikipedia. 

Examples:

- Performance 
- Scalability
- Portability 
- Compatibility 
- Reliability
- Availability
- Usability
- Maintainability
- Security
- Localization


#### Constraints

Another overlooked aspect which can throw away a solution are the constraints the stakeholder has. 

These can come up as a surprise late in time as well, therefore exercising the stakeholder's mind to start think about the constraints as well during the initial phases can come in both parties benefit. It can reduce drastically the element of surprise later in time, as well as the misunderstandings.

The constraints questions can be difficult to identify, as they very much depend on the problem statement. 

Some examples of areas for constraints can be:

- the stakeholders budget and timelines

- the stakeholders preferences with regards to technology stack or cloud provider

- the stakeholder need and expectations for maintaining the solution

- if there are any integration points, then sky is the limit with the amount of constraints which can be identified in this phase, or which can be taken later on in the process.

- GDPR

- etc

### A small tip

A small tip  before proceeding to the next phase is to define together with the stakeholders a Glossary. This can reduce the level of misunderstandings, and it will help you start talking in the business terms of your stakeholder, which will bring huge level of appreciation.

### Phase 2 - Designing the solution for the MVP

The next phase is starting to design the technical solution. This part is covered by System Design books and practices.

Taking into consideration the needs for the MVP, trying to reduce the complexity as much as possible so that the first draft can see the light as fast as possible. 

The small guidelines I would offer here for a structured approach would be:

- consider only the limited amount of use cases for the MVP

- take into consideration both the failure and the success of the product

- there can be situations when only proving something fast can be more valuable than making it perfect and extensible for enhancing it with future needs

- don't focus on the technologies yet, just identity the domains and the borders for a natural split of responsibilities 

- add the technologies only at the end( it can be the case when a specific technology is being asked from the beginning, and can influence the solution, if yes, then this and previous point can be merged)

- ask for feedback from as many close peers you can

There is plenty of literature on the internet from books, articles, certifications, videos or training websites in order to develop Systems Design skills for being able to prepare a concept. But practice makes it all. 

But as a short summary of aspects which have to be taken into account, at this step, three questions have to be answered:

- What are the structural elements of the system?
- How are they related to each other?
- Why: What are the underlying principles and rationale that guide the answers to the previous two questions?

The context, the constraints and the history can play a huge role.


### Phase 3 - Break down the solution

The level of the detail required here would be the one which can make sense, to transform solution in something tangible, concrete with clear steps.

The structure can vary based on needs. The most extensive one I used was:

- Use Case - for bridging the initial discussion to the technical world, it can be viewed as one Epic
- Use Case Description 
- Technical Component - which refers to the box from the system design
- The Story - which contains the breakdown of the smaller steps for reaching out to the completion of the epic

As here we are talking about technical tasks, depending on how your company works, make sure to take into account the non visible work aspects:

- Environments preparation
- Testing
- CI/CD
- Monitoring


### Phase 4 - Risks Management

**"If you can’t think of three things that might go wrong with your plans, then there’s something wrong with your thinking"(Jerry Weinberg)**

While writing each of the part from the previous step, take into account that everything can be challenged.
Preparing to justify you reasoning helps by having a list of **assumptions** for each side. They will reflect your understanding and what was taken into consideration. Will help identify if things were missed in the past get better prepared for the future.

The Risks are the second very important aspect while going through the list, as they can highlight things which might go wrong. Or even things you don't know and have to be discovered. They shall help you build the list of **risks**



### To POC or not to POC

Depending on the time you have, the clarity of the solution and depending on the requested level of accuracy, POC might come in handy for clearing up parts of the unknown, and bringing confidence in the proposed solution.


### Phase 6 - Estimates

But first keep in mind that **"It's expensive to  know everything up front"(Kolton Andrus)**

The theory of estimates is vast, as each of the above topics, it ca be dissected into its own blog post. But I am very conscious that it is always a trap. A trap of lies, misunderstandings and misbelieves. People overvalue their knowledge and underestimate the probability of their being wrong.

It always helped me stay grounded by using the three point estimation. It can be cumbersome, but helped me a lot by covering the above structure in a methodical manner which could bring the overall estimations to a more realistic ground.

- The Best Case estimates - shall cover the obvious defined stories while taking the assumptions into account

- The Most Probable estimates - shall bring you more to the grounds, taking into account that people who were never involved in the initial discussions can work on the topics, while their level of experiences can be of various degrees 

- The Worst Case estimates - shall cover the identified risks, to be able to sustain them with concrete things which might go wrong.


Therefore, keep in mind:

- educate your unexperienced stakeholders, for setting the correct expectations; at this stage the level of accuracy can be very low

- with more experienced stakeholders, who went through this before, at this level most of the times what matters the most is only giving a hint on the number of zeros they are facing ahead (if your talking about months / years ...)

- a too realistic number can scare stakeholders away; I speak from experience; without the right relationship and the common understanding of things, a huge number can make customers shut doors 

- a smart way but transparent of stripping the complexity to minimum could help both parties while also keep it grounded


### Phase 7 - Costs

On this topic I cannot advise much. As many companies there are, as many rates and different strategies. 

### Conclusion

If you found at least a small idea useful from above I am happy. For any more questions, resources requests or suggestions, please feel free to approach me.


**References** 

[Rational Unified Process](https://en.wikipedia.org/wiki/Rational_Unified_Process)
 
[MindTools](https://www.mindtools.com/pages/main/newMN_TMC.htm) 

[What is Problem Solving](https://www.mindtools.com/pages/article/newTMC_00.htm) 

[Software Development Process](https://en.wikipedia.org/wiki/Software_development_process)

[Non-Functional Requirements](https://en.wikipedia.org/wiki/Non-functional_requirement)

[System Quality Attributes](https://en.wikipedia.org/wiki/List_of_system_quality_attributes)

[Lies Damn Lies Estimates](https://www.slideshare.net/sebrose/lies-damn-lies-estimates)

**"The moment design becomes important is when you want to change something"(Kent Beck)**

