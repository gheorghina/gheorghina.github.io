---
layout: post
title:  "Solving Business Problems, a View from a Technical Leading Perspective"
date:   2023-07-16 00:18:23 +0700
categories: [lead dev, leadership, architecture, digital change]
---

When embarking on substantial integration software projects or any other endeavors characterized by elevated levels of uncertainty and risk, especially when there are no clear requirements in sight, there are several best practices and steps that a technical leader (Principal Software Engineer, Engineering Manager, Staff Software Engineer, Software Engineer, or anyone with the requisite skills) can follow.

The ultimate goal to keep in mind and strive for is to transform uncertainty into actionable outcomes, enhance visibility, and improve transparency for various stakeholders from both the product and technology sides.

This blog post will define the key steps and artifacts defined by the well known software development process RUP. At the end of the blog post, the second well known development process, [Systems Development Life Cycle(SDLC)](https://en.wikipedia.org/wiki/Systems_development_life_cycle) will be discussed in comparison to RUP. 

### [Rational Unified Process(RUP)](https://en.wikipedia.org/wiki/Rational_unified_process) 

RUP is an iterative software development process framework created by the Rational Software Corporation, a division of IBM since 2003, and has since become one of the most widely used software development methodologies.

RUP is not a single, concrete, prescriptive process but rather an adaptable process framework intended to be tailored by development organizations and software project teams. These teams can select the elements of the process that are suitable for their specific needs. RUP is a specific implementation of the Unified Process.

The three key elements that define RUP are:

1. Software Development Guidelines: These provide a foundation for success.
2. A Framework: This encompasses reusable building blocks for method content and processes from which customized procedures and method setups can be created.
3. A Language: It describes methods and processes.

### Tailored minimum key practices and steps

1. **A Cross-Functional Team** Establish a cross-functional team: Assemble a team comprising of members from different domains such as engineering, product management, quality assurance, and operations. This will ensure that the team has the necessary skills and expertise to deliver the project.

Depending on the nature of the problem, broader experience can help reduce the uncertainty, get clarity in shaping the requirements, speed up the shaping process and ensure the buy-in of the final solution of various stakeholders.

2. **Team Meeting Frequency** Decide the cadence of meeting for aligning on both findings but also the next steps: weekly, bi-weekly, online/on site workshops(with the involved parties)

3. **Define the [RACI Matrix](https://en.wikipedia.org/wiki/Responsibility_assignment_matrix)**. As per wiki: "A responsibility assignment matrix(RAM), also known as RACI matrix or linear responsibility chart (LRC), describes the participation by various roles in completing tasks or deliverables for a project or business process. RACI is an acronym derived from the four key responsibilities most typically used: responsible, accountable, consulted, and informed. It is used for clarifying and defining roles and responsibilities in cross-functional or departmental projects and processes. There are a number of alternatives to the RACI model." 

It is used for clarifying and defining roles and responsibilities in cross-functional or departmental projects and processes. This is a great tool for bringing clarity on who will do what with regards to the project. This will reduce the misunderstanding, stepping on each other toes and will ensure the informed parties or other stakeholders are clarified.

4. **Stakeholders Management** Identify and engage key stakeholders: Identify all relevant stakeholders, including both product and technical stakeholders. Engage them early on and throughout the project to ensure their concerns and expectations are understood and addressed.

5. Decide on the cadence of informing the stakeholders and the format(email, status snapshots slides, monthly steer-co sessions)

6. **Single source of truth for the project documentation** - Create a project workspace which shall have the goal to centralize as single source of truth the entire documentation and process based on a tailored version of the standard software development process framework RUP.

*At this stage we are interested in focusing on the first two phases definitions from RUP: Inception and Elaboration*

7. **Define the project vision and the goals**: Work with the stakeholders to establish a clear vision alignment and high-level goals for the project. This will provide a guiding direction for the team and ensure the initial alignment. 

    *Depending on the complexity or level of uncertainty of the project, if the product vision details cannot be defined, it is important to compile a strategy plan on how to get to the Vision definition. This is a good enough exercise to align the team members and the stakeholders on what will be the next steps.*

8. **Discovery phase** - Dedicate time for a discovery phase to explore the problem space, understand the business context, the user needs and identify potential risks and challenges. The outcome of this phase could be a project charter or a discovery report summarizing the findings.

    Various team members will have a different role and purpose through the discovery phase, but the end goal is to bring clarity on the WHAT and on the HOW to the level of details that a scope can be shaped for implementation. 

9. **Define a glossary** This component is key to ensure that the involved parties refer to the same aspect while using a given terminology.  
The lack of having a glossary can introduce a considerable degree of misunderstandings and misalignments, leading to an unwanted increased solution complexity. 

10. Document the identified **business use cases**, **business process models**, **business process flows**. 

    Expected Artifacts:

    - Business Process model / flow or activity diagrams
        - [Activity Diagrams](https://en.wikipedia.org/wiki/Activity_diagram)
        - [Flow Charts](https://en.wikipedia.org/wiki/Flowchart)
    - Product Requirements Documents defined at the epic level. A guideline template is provided below.

11. If the solution is not completely new, but built on an existing solution, then documenting the high level **Architecture System Status Quo** and **The Context Details** is a must. As the new solution will be built on top of the existing one, the project plan in the end will have to reflect how one can get from the existing status quo to the vision.

12. Define a minimum viable product (MVP) scope: Develop an MVP that addresses the most critical aspects of the integration software project. The MVP should deliver value to users and help validate assumptions and requirements.

13. For the aligned and defined reduced scope in the PRD, document the expected systems architecture, design decisions, and technical specification details.

    Expected Artifacts for the new **Architecture System Vision**:

    - [C4 model System Design](https://c4model.com/)
    - [Sequence Diagrams](https://en.wikipedia.org/wiki/Sequence_diagram)
    - [Integration Diagrams](https://en.wikipedia.org/wiki/System_integration)
    - [Deployment Diagrams](https://en.wikipedia.org/wiki/Deployment_diagram)
    - [Testing Strategies](https://en.wikipedia.org/wiki/Test_strategy)
    - Any other diagrams which may bring clarity and faster alignment

14. **Risk Assessment, Documentation and Management**

    Risk management is an important function in the organizations today. Nowadays, the undertaken projects are increasingly complex and ambitious. They must be executed successfully, in an uncertain and often risky environment.

    The good news is that the risk management process is not a new concept, there are frameworks which can be used. One only must build a sensible eye and mind for identifying the risks and do the due diligence related to managing them.

    Recommended resources to read:

    - The minimum Risk Management Matrix for defining a Risk Profile:
        - Risk
        - Probability(high, medium, low)
        - Impact(high, medium, low)
        - Mitigation Plans: solutions to avoid the risk from happening
        - Contingency Plans: solutions to cope once the risk materializes
        - Reporting and Monitoring solutions

    - [mindtools](https://www.mindtools.com/ah8ju2z/risk-impactprobability-charts) 

    - [RMF by techtarget](https://www.techtarget.com/searchcio/definition/Risk-Management-Framework-RMF)

    - [NIST Risk Management Framework](https://csrc.nist.gov/projects/risk-management/about-rmf)

15. **Roadmap plan**: Define the roadmap plan. This is a joined effort between tech and product to ensure alignment on how the teams will move further.

It can be that the scope is to big, so prioritization or split in multiple phases may be necessary. On top of that it can give clarity on the expected checkpoints throughout the delivery process.

16. Schedule retros for allowing the chance to have feedback sessions. These are helpful for calibration with the team before moving further.It is important for creating the open feedback culture to encourage growth and support of each other. 

17. Reiterate and Adapt

18. Execution

    Adopt an iterative approach: Break down the project into smaller, manageable increments or iterations. Each iteration should deliver a usable and testable portion of the software. This iterative approach allows for frequent feedback loops and course corrections.

    Use agile methodologies: Consider adopting agile methodologies, such as Scrum or Kanban, to enhance collaboration, flexibility, and adaptability. Regularly conduct sprint planning, daily stand-ups, sprint reviews, and retrospectives to improve team communication and productivity.

    Implement continuous integration and delivery: Set up automated build, test, and deployment pipelines to ensure that changes are integrated and delivered frequently, reducing the time-to-market and enabling faster feedback cycles.

    Maintain comprehensive documentation: Document the project requirements, architecture, design decisions, and technical specifications. This documentation serves as a reference for the team and future stakeholders.

    Communicate progress and risks: Regularly communicate project progress, risks, and challenges to all stakeholders. This could be achieved through status reports, project dashboards, or regular meetings. Transparency builds trust and helps manage expectations.

    Foster collaboration and knowledge sharing: Encourage collaboration within the team and across teams. Foster a culture of knowledge sharing through code reviews, pair programming, and documentation reviews.
    
### Product Requirements Document Template

A product requirements document should be clear, concise, and easily understandable by both the development team and stakeholders. Regular collaboration and communication between product managers, stakeholders, and the development team are essential to refine and clarify the requirements as the project progresses.

A PRD shall contain at least the following sections:

1. Introduction:
    - Overview: Provide a high-level overview of the product, its purpose, and the problem it solves.
    - Target Audience: Describe the intended users or customers for the product.
    - The problem or opportunity we are trying to solve
    - High level approach taken to solve the problem or the opportunity

2. Glossary:

    Include a glossary of key terms and definitions used in the PRD to ensure clarity and consistency.

3. Product Goals and Objectives:

4. Goals: Clearly state the overall goals and objectives of the product.

5. Key Success Metrics: Specify the measurable criteria for success or performance.

6. Functional Requirements:

    - Use Cases or User Stories: Describe typical user interactions and workflows with the product.
    - Feature List: Enumerate the specific features and functionalities the product should have.
    - User Interface (UI) Design: Include wireframes, mockups, or design guidelines for the UI elements.

7. Non-Functional Requirements:

    - Performance: Define performance requirements such as response times or throughput.
    - Scalability: Specify the expected scalability needs as the user base grows.
    - Security: Identify any security requirements, authentication mechanisms, or data protection measures.
    - Compatibility: Outline compatibility requirements with specific platforms, devices, or browsers.
    - Resiliency: Ability to cope with failures
    - Usability: Describe any usability guidelines or requirements for the product.

8. Technical Requirements:

    - Architecture system design 
    - Technical detail description per component and per integration point
    - Hardware and Software Dependencies: Specify any required hardware components or software dependencies.
    - Integration Requirements: Identify any integrations with third-party systems or APIs.
    - Data Requirements: Define the data structures, databases, or data sources needed by the product.

9. Assumptions and Constraints:

    - Assumptions: List any assumptions made during the requirements gathering process.
    - Constraints: Highlight any limitations or constraints that may impact the product's development or functionality.

10. Risks Management Matrix

11. User Acceptance Criteria

    - Define the criteria that need to be met for the product to be considered acceptable or ready for release.
    - Specify any specific test cases or scenarios that should be used for user acceptance testing.

12. Testing Strategies

13. Out of scope items, to avoid future misalignments

14. Project Timeline and Milestones:

    - Provide an overview of the project timeline, including major milestones and deliverables.
    - Note any dependencies or critical path items that may impact the schedule.
    - Add priorities to each step to highlight even further the order in which the items can be tackled.

15. Left outside but important to be considered once the project starts:

15.1 User Manual Definition
15.2 Go to Market Strategy
15.3 Metrics, Analytics
15.4 Deliverables Quality Insurance

### [Systems Development Life Cycle(SDLC)](https://en.wikipedia.org/wiki/Systems_development_life_cycle) 

As per wiki:

In systems engineering, information systems and software engineering, the systems development life cycle (SDLC), also referred to as the application development life cycle, is a process for planning, creating, testing, and deploying an information system.

The SDLC concept applies to a range of hardware and software configurations, as a system can be composed of hardware only, software only, or a combination of both.

SDLC is not a methodology per se, but rather a description of the phases that a methodology should address. The list of phases is not definitive, but typically includes planning, analysis, design, build, test, implement, and maintenance/support.  

### RUP vs SDLC

The main difference between the Software Development Life Cycle (SDLC) and the Rational Unified Process (RUP) lies in their approach and level of flexibility. Let's explore these differences:

1. Approach:

**SDLC**: The SDLC is a broad framework that outlines the general stages and activities of software development, from requirements gathering to maintenance. It provides a sequential, linear approach to software development, where each phase is completed before moving to the next. The emphasis is on planning and executing each phase in a structured manner.

**RUP**: RUP, on the other hand, is an iterative and incremental software development process framework. It focuses on shorter development cycles and feedback loops, enabling iterative development and continuous refinement of the software. RUP emphasizes flexibility and adaptability to accommodate changing requirements and emerging risks throughout the project.

2. Flexibility:

**SDLC**: The SDLC is often associated with a more rigid and inflexible approach. It follows a fixed sequence of phases and typically involves a waterfall model, where each phase is completed before proceeding to the next. Changes to requirements or design during later phases can be difficult to accommodate without going back to earlier stages.

**RUP**: RUP, being iterative and incremental, offers greater flexibility. It allows for feedback and adjustments at each iteration, enabling the project team to adapt and refine requirements, designs, and deliverables based on emerging needs and insights. RUP encourages embracing change throughout the project.

3. Artifact Focus:

**SDLC**: The SDLC primarily focuses on the development phases and the artifacts produced during each phase. It provides general guidelines on what artifacts should be created, such as requirements documents, design documents, and test plans. The emphasis is on delivering predefined artifacts at each stage.

**RUP**: RUP also focuses on artifacts, but it places greater emphasis on the iterative development and refinement of artifacts throughout the project. RUP provides guidelines for producing artifacts specific to each iteration, such as updated use cases, revised system architecture, and evolving design documents. The focus is on delivering incremental value with each iteration.

4. Process Tailoring:

**SDLC**: The SDLC is often seen as a more generic framework that can be tailored and adapted to suit the needs of individual projects and organizations. It provides a foundation for defining project-specific processes, incorporating specific methodologies or techniques as required.

**RUP**: RUP is a specific process framework that provides a more detailed and prescriptive set of practices, guidelines, and artifacts. While it can be tailored to some extent, RUP already defines a comprehensive set of practices, artifacts, and roles, making it less flexible in terms of process customization.

In summary, the SDLC is a broader framework that outlines the general stages of software development, while RUP is a specific iterative and incremental process framework. RUP offers greater flexibility and adaptability, with a focus on iterative development, continuous refinement, and accommodating changing requirements. The choice between SDLC and RUP depends on the project's needs, complexity, and the desired level of flexibility.

### Additional References

- [Software Development Life Cycle (SDLC)](https://cio-wiki.org/wiki/Software_Development_Life_Cycle_(SDLC))

- [Software Architecture](https://cio-wiki.org/wiki/Software_Architecture)

- [c4 model](https://c4model.com/)