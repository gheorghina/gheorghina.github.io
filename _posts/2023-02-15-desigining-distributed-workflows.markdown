---
layout: post
title:  "Designing Distributed Business Process Models"
date:   2023-02-15 00:18:23 +0700
categories: [workflows, distributed workflows, scalability, resiliency, orchestration, choreography]
---


## Designing Distributed Workflows

Once moving away from a monolithic solution to distributed systems, the collaboration and coordination between various domains increases in complexity. In order to enable more domains to contribute to an outcome, depending on the level of coordination required, two fundamental patterns can be considered for the services collaboration models: orchestration or choreography.

Each pattern has its own distinct characteristics, benefits, drawbacks, and recommended use cases.

The choice between orchestration and choreography depends on the specific requirements of the system and the trade-offs that need to be made. 

Orchestration offers centralized control and easier management but introduces a single point of failure and coordination overhead. 

Choreography provides decentralized autonomy and loose coupling but lacks global visibility and can be complex to debug. 

Understanding the characteristics and considering the pros and cons of each pattern can help architects and developers make informed decisions about which approach to adopt in their microservices architecture.

## Architecture Principles

There are a few key architecture principles which have to be taken into account for building the capabilities of shaping workflows in distributed systems.

These principles are meant as guidelines which can simplify the process and reduce the complexity of dealing with multiple systems:

0. Each system shall be the single source of truth for one particular domain boundary. This is a general best practice while dealing with distributed systems. This principle keeps communication clear and the systems dependencies small. It simplifies the architecture decisions and technical solutions in general.

1. Ideally, the amount of workflow states shall be kept small with focus on the global view. The details on how each system reaches the state shall be abstracted away.

2. The systems using the high level checkpoints shall be able to cope with eventual consistency. As the sources of truth may be different from the system which gives the overall system status.

3. Ideally, each new system shall have full autonomy on managing its internal subdomain. The details of the subdomain, have to be hidden away from the global view of the overall system status.

4. The workflows shall be mainly responsible for managing global states, or global impact of changes. Changes within on system while reaching a global state, must be abstracted away.

5. Idempotence becomes a good factor for consideration, as it is important to be able to recover from various failures. The ability to cope with retries in case of failures or partial degradation becomes vital for the resiliency of a mechanism.


## A few NFRs to be taken into account

- Performance

- Resiliency

- Observability

    - Ability to identify if the flow is stuck, the reasons for it, and have the ability to unblock itself

- Ability to cope with eventual consistency 

- Flexibility

- Standardized way of integrating systems

- Ability to manage distributed transactions as a last resort if required


## Designing Workflows with Sync/ Async Orchestration 

![]({{ site.url }}/static/img/distributed-workflows/Orchestration.jpg)

Orchestration is a communication pattern where a central entity, known as the orchestrator, controls and coordinates the interactions between microservices. 

The orchestrator is responsible for initiating and managing the workflow of the overall process, directing microservices to perform specific tasks in a predefined order. 

This pattern is typically implemented using a centralized service or a dedicated orchestrator component.

#### Pros:

- Ability to have a centralized place for coordination, which can be put in place in a configurable, standardized manner. The coordination can be across systems, across domains, across companies, with the ability to accommodate human interactions shaped as a triggers for various points coming from various sources.

- Microservices can focus on their specific tasks without needing to know about the overall process, promoting loose coupling between components.

- Increased observability capabilities by having an enhanced visibility on the status of a flow
  
- Support for resiliency and retry mechanisms in case of failures. This brings to an increased recoverability in case of failures.

- With standard configuration based integrations, flexibility and independence can be put in place for building different versions of other workflows.

- Controlled reconciliations in cased of changes which have to be accommodated across the system  

- Fault tolerance due to service coupling can be mitigated by using async communication in orchestration as well.

- Ability to build powerful flow control constructs (decisions, dynamic forks or design of sub-workflows)

- Ability to introduce centralized notification systems which respond to various checkpoints for increased visibility and flexibility to the events sent out.

- Changes to the process can be easily made by modifying the orchestrator's instructions without affecting the individual microservices.

#### Cons:

- The orchestration platform can become a bottleneck and a single point of failures if it is not handled properly with the necessary performance, flexibility, resiliency and redundancy support.  If the orchestrator fails, the entire process can be disrupted.

- Complexity in ensuring scalability. Local scalability will have to be taken into account. Implementing and maintaining an orchestrator adds complexity to the system architecture.

- Centralized dependency for the systems integrated in the flow. The orchestrator needs to handle communication and coordination between microservices, which can introduce additional network latency.

- This communication style doesn’t scale as well as choreography because it has more coordination points (the orchestrator), which cuts down on potential parallelism. 

- Service coupling: Having a central orchestrator creates higher coupling between it and domain components, which is sometimes necessary.  

#### When to use it:

- In case of expectations to manage additional alternative paths to the happy flow(errors, edits). In case there are many additional exceptions which have to be coordinated to a happy flow (a good candidate when on top of a happy flow there is a need for edits and reconciliations support)

- Long-running processes: Orchestration is suitable for processes that span multiple microservices and involve complex coordination.

- Transactional workflows: When maintaining consistency across multiple microservices is critical, orchestration can ensure atomicity and integrity.

## Designing Workflows with Choreography

![]({{ site.url }}/static/img/distributed-workflows/Choreography.jpg)

Choreography is a communication pattern where each microservice plays an active role in the collaboration, and interactions are decentralized. 

Each microservice communicates directly with other services using events, messages, or shared data schemas. 

There is no central control or coordinator dictating the workflow; instead, microservices autonomously decide how to respond to events or messages they receive.

#### Pros:

- Without a centralized orchestration point, the architecture design can be built with more opportunities for parallelism, making the entire solution more responsive.Microservices can act independently and make their own decisions based on received events, allowing for greater scalability and flexibility.

- Without a centralized orchestration point, a more independent scaling can be considered making the system more scalable and more fault tolerant. Services are loosely coupled as they only need to know about the events or messages they interact with, enabling easier maintenance and scalability. 

- Since there is no single point of failure, the system can gracefully handle the failure of individual microservices.

- Easier to introduce new systems as long as the integration models are standardized. 

#### Cons:

- In choreography there is no obvious owner for the global visibility. The state of the system will be spread across the system. With no central entity coordinating the process, it can be challenging to get a global view of the entire flow. Observability gets even more complicated when multiple flows with forks and sub-flows have to be supported. 

- Not having a centralized state holder hinders ongoing state management.

- Maintaining the consistency and order of events can be complex, especially in highly distributed systems.

- Debugging and troubleshooting can be more difficult in a decentralized environment as events and messages flow through multiple microservices.
 
- The workflow steps would end up being distributed which leads to increased difficulty in assessing the status of the system, failures identification and recovery in case of errors.

- End-2-End observability of a current state snapshot gets more difficult to obtain. In this case distributed tracing mechanisms become vital for a successful integration and incident assessment. 

- Complex edit scenarios have to be implemented as group of actions and events, which can lead to more complex mechanisms, collaboration and alignment between teams

- Complexity in adding fast different versions of workflows. As the state transitions and impact would be implemented in a choreography manner, it is difficult to control the standardization in the mechanisms implementation in each service. This can make it very difficult to have to introduce management of different order of events for different use cases. This usually can bring a system to spaghetti code and increased complexity in coping with changes.
 
At first glance, the choreography solution seems simpler—fewer services (no orchestrator), and a simple chain of events/commands (messages). However, as with many issues in software architecture, the difficulties lie not with the default paths but rather with boundary and error conditions.

- Error conditions in choreography typically add extra communication links

#### When to use it:

- When the system is designed around asynchronous event-based communication, choreography can provide a natural fit.

- When microservices are developed and maintained by separate teams or organizations, choreography allows for autonomy and loose coupling.

- When most of the above orchestration use cases are not met.


## Design Principles for Ensuring Flexibility

Irrespective of the communication model, it is vital to ensure a standardized manner for shaping integrations so that increased visibility and transparent communication can be put in place for the interaction points.

In order to shape abstracted integration, the activities and entities shall be split in generic categories.

Examples:

- Configurations 

- Triggers

- Validations

- Actions / Tasks

- Impacted systems 

- Variables support

- Messaging Based Communication: Pub/Sub; Command/CommandResponse mechanisms

For performance and resiliency support it is advisable to abstract and separate the activities which can be parallelized via various mechanisms. 

For long running processes the async approach ensures immediate retake of activities once dependent activities are completed. 


## Existing Solutions in the Market (if you don't want to build your own)

- [Camunda](https://camunda.com/)

- [AWS Step Functions](https://aws.amazon.com/step-functions/?step-functions.sort-by=item.additionalFields.postDateTime&step-functions.sort-order=desc)

- [Apache Airflow](https://airflow.apache.org/)

- Use frontend apps as orchestrators

- [Conductor](https://conductor.netflix.com/)


## References

- [Software Architecture: The hard parts](https://www.goodreads.com/book/show/58153482-software-architecture?ac=1&from_search=true&qid=pe9Z1j918i&rank=1)
- [Manage Distributed Locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html) 
