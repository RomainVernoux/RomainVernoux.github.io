---
layout: post
title: "The road to microservices"
date: 2021-02-28
categories: architecture
tags: english
---
Tech conferences piling up presentations from large cloud-native companies, shiny new frameworks, consulting firms building up their portfolios, software engineers discussing how to ride the hype at the coffee machine... Microservices are everywhere.
A lot has already been said and written about them, often making microservices look like a silver bullet.
But why leave the comfort of the well-known, sometimes layered, sometimes modularized, monolithic architecture?
Do you really need what microservices offer? Are you really ready to lose what microservices sacrifice?
This article tries to summarize my view on the "microservices compromise", what it requires and what it implies, beyond splitting a codebase into small pieces.  

For the road to microservices is dark and full of terrors.

<!--more-->

## The promise

On paper, microservices architectures offer a lot of benefits...

### Small, focused and decoupled services

It is known that small systems are easier to reason about and build. Who has never noticed that software is never produced more efficiently than during the first few weeks of development?
This also holds for maintenance: regardless of team seniority and turnover, it is always easier to keep alive and change behavior in a small codebase than in a large plate of spaghetti code.
Eventually, all software is retired, either because too many mistakes were made and the team decides to start over, or because the requirements or the environment changed too much. 
Again, replacing multiple small services is easier than redesigning an entire software, but the main benefit remains that it can happen progressively, over the course of multiple months, and without blocking other development efforts.
The times of "we need to stop adding new features to the product to deeply refactor the codebase foundations" are over.

Good news then! Microservices architectures are all about splitting software in multiple services and keeping them small.
When done right, this code split follows the lines of the various concerns handled by the application.
This means that each service is focused on a small, specific set of features, and does not have to care too much about the rest of the application logic.
The benefits of such a decoupled architecture are obvious: ease of development and evolution, fewer dependencies between teams and roadmaps, fewer bugs and anomalies... 

### Technology heterogeneity

In a monolithic architecture, the choice of language and framework is made once, usually at the very beginning of the project.
Librairies tend to accumulate as new requirements arrive or because team members change, and so do their preferences in tooling.
This is why all monolithic architectures usually go for a large, generic framework (e.g., Spring for Java codebases) and a standard storage solution (e.g., a relational database).

*"Jack of all trades, master of none"*, as the saying goes.
Despite all their (often impressive) efforts, how could such frameworks provide, at the same time, the optimal solution for use cases requiring a fast startup (async tasks), best performance during a few minutes or hours (batch jobs) and everything (from high frequency/low complexity to low frequency/high complexity) in the spectrum of modern web application workloads?
How could a database provide, at the same time, the best options to store structured, unstructured and binary data, regardless of response time, volume, scaling and consistency requirements?

The issue does not stop there, and soon enough a new, modern framework will appear that looks better suited for the job at hand.
At this point, there will be no good option anymore: should you stick with the old, eventually abandoned framework, or rewrite the entire application? You will be stuck.  

In microservices architecture, the rules are different.
You can choose the right stack (language, frameworks, librairies, storage...), at the right moment, on a use case (microservice) basis.
This could mean using, at the same time, a compiled language with no framework (e.g., Go) for services for which startup time is crucial, a lightweight web framework with an efficient, distributed cache (e.g. Node, Express and Redis) for high frequency/low complexity workloads, and an advanced framework in a typed, interpreted language (e.g., Java and Spring) for low frequency/high complexity use cases.
The same logic applies to storage, and microservices also open the door to using, for instance, a columnar database alongside a relational database for analytics and transactional workloads.    

### Resilience and scalability

Monoliths offer what I call "all-or-nothing" availability. Taking aside a few uncommon cases, the application is either up and running, or completely down.
This situation is quite easy to reason about, since the resilience of the system boils down to your ability to keep the monolith process running.
If anything goes wrong (host failure, crash, network failure, insufficient disk space...), the whole application is gone.

With microservices, a failure only affects one or a few of the multiple parts of the system, leaving most of the services running.
This means that you now have the opportunity to react to partial failure and expose a degraded service to your users.

Regarding scalability, monoliths can only grow vertically.
That is, the only way to make a monolith faster or handle more requests is to make it run on a faster or bigger host.

Microservices, on the other hand, allow for multiple scaling strategies, which can even be combined:
- vertical scaling: memory and CPUs are added to make things go faster
- horizontal scaling: the load is shared between multiple identical instances behind a load balancer
- sharding: the load is split between multiple instances depending on the data it needs to access
- functional decomposition: the service is split into smaller services with technologies and scaling strategies specific for each kind of load

Of course, all scaling strategies are not always available. For instance, horizontal scaling requires the service to be stateless, and sharding requires the computation to be localized enough for the underlying data to be sharded. 
One could also argue that some of these scaling strategies are also possible with monoliths, but to me this is already a step on the road to microservices!

In any case, the take-away is that, in microservices architectures, the resilience and scaling strategies can be chosen independently for each service: a batch microservice could scale vertically and not have resilience requirements beyond a "retry later tomorrow", while a web server is made to use sharding and scale horizontally.
None of these options will ever be available in a monolithic architecture.

Finally, keep in mind that resilience and scalability decisions do not only pertain to services your team develops, as other components (databases, message brokers, API gateways...) come with their own tradeoff and scaling options, with a definitive impact on the behavior of the overall system.   

### Teams and organization

As the size of a software grows, the size of the corresponding team tends to grow accordingly, often with the incorrect assumption that more people implies more code delivered.
It is well documented that the effort required to synchronize team members is not linear in the number of people, but rather quadratic: to synchronize 2 people, 1 phone call is enough ; to synchronize 5 people, 20 one-to-one phone calls are required.
Large meetings only solve this issue up to a point, arguably around 5 to 10 people.

Microservices offer the opportunity to naturally split the work between small teams, each focused and knowledgeable about their microservices and the few they interact with.
Although collaboration and synchronization points between teams is still required, the frequency and length of such events should be dramatically reduced.

## The untold story of the compromise

After reading the previous section, you will probably really want to use microservices in your current project and all the other ones, past and future!
But as everybody knows, there is no silver bullet, or no free lunch. Only compromises.
So let me sing you a song that is rarely sung, the one about the road to microservices, and everything you lose along the way!

### Small and coupled

Models in monoliths are not created as a large pile of coupled objects by design.
They progressively but inexorably get there, because concepts in the real world are inherently coupled, and simplifying reality to extract and maintain a simple, decoupled model over time is no easy task (it can actually be a full-time job).
In monoliths, coupling can stay unnoticed for quite some time before the first symptoms appear, affecting either development velocity, ease of maintenance or performance.
In microservices, there is nowhere to hide. With parts of the model distributed across the network, the cost of coupling is orders of magnitude higher, and will almost instantly result in maintenance burden and performance bottlenecks.
A lot more effort will also be required to fix the model: instead of refactoring a local set of objects, multiple services will now need to be merged, split and rearranged, with a global impact on the architecture.

Do not expect decoupling to come for free from splitting a large codebase into small pieces.
In fact, it works the other way around: the split into small pieces will only work if your team already has the ability to design a decoupled model.
If your monolithic codebase looks like a plate of spaghettis, it is therefore a strong hint that you lack the modeling skills required to succeed in microservices.

With microservices, the model now has boundaries that are hard to move, and the possibility of a large scale redesign is essentially lost.
If you get these boundaries wrong, you end up with a lot of small, coupled services, which is absolutely worse than a large, coupled model in a monolith.

So here is the compromise: how confident are you in your ability to extract and maintain a useful, simple, decoupled model? How much splitting into small pieces are you willing to bet on it?

### The technology zoo

Like in everything else, flexibility in technology options is great. But it comes with a price.
Instead of one technology stack to master, you now have two, three, five, ten of them, each possibly made of multiple languages, frameworks, librairies and tools!
And we are not only talking about development skills, but rather about a company-wide matter, from development and testing to integration, build tooling, deployment, configuration, monitoring, performance management, security...
Having multiple technologies in production means having to maintain a pool of experts for each of them, in each department. Otherwise, the next rewrite from scratch will not be because of code quality but because of a lack of skills to implement simple changes.

Letting everyone use the languages and tools they like and hope for the best is clearly not a valid strategy, and some guidelines must be established.
They can take the form of a reduced set of technology stacks to choose from (e.g., Netflix seems to focus on JVM technologies), or a particular process to test, validate and train people on new languages or frameworks. 

Too many constraints, and you don't get the advertised benefits of technology heterogeneity; too few, and the fun in the technology zoo will not be for long!
Quite obviously, this is again a matter of compromise.

### Cascading failures and stateful services

Compared to monoliths and their all-or-nothing availability, microservices bring a different equation to the table.
Because they are composed of more moving parts (multiple hosts, multiple services, multiple network links between them...), they are statistically more likely to fail.
Pushed to a certain scale, a microservice architecture is, at any point in time, statistically more likely to have at least one failing component than to be entirely healthy.
Seriously, do the math. Assume a 99% overall uptime for all your components:
- with only one component, the probability to be healthy is, well... 99%
- with *N* components, the probability to be healthy is 99%<sup>N</sup>
- 70 components are enough to have a <50% chance to not have any failure at a given time!

Microservices actually bring more failure, not less.
But they also bring the opportunity to keep them localized and compensate for them.
But again, it does not come for free, and there extra work to be done to get there!

The first solution that should come to mind is the *Bulkhead* pattern, in which multiple, identical instances of a service are used behind a load balancer to handle traffic.
If one of the instances crashes, the load balancer stops sending traffic to it until it recovers (in some advanced versions of this pattern, the failing instance is automatically killed, and another one is spawned somewhere else).
Beyond the infrastructure and configuration work implied here, there is also a hidden requirement that the service handling traffic needs to be stateless.
If this is not the case, you also need to invest there. Of course, this is just for one pattern ; other ones (for instance, *Circuit breaker*), come with their own hypotheses about your system, and implementation burden.

In any case, keep in mind that unhandled errors have a greater impact in distributed systems, as you cannot simply rely on the nice transactional properties of a shared database.
Without transactions, if one thing fails in one service, you have to explicitly undo something else in another service. And what if this other action fails?
Some patterns, such as *2-Phase Commit* or *Sagas*, can help here, but they also require extra work and come with their own trade-off, especially regarding coupling.
  
So, in the end, do you prefer investing in patterns to fight against the cold reality of statistics, or reducing the number of moving parts? Who said "compromise"?

### Transversal teams and change management

Code is easier to change than organizations or culture. And cutting the code into small pieces will not magically change the way people work together.
If your entire enterprise is organized around technical boundaries (e.g., a frontend team, a backend team, and an ops team), massive efforts will be required to evolve to a lot of small, focused, multidisciplinary teams.
Imagine, 10 teams with each their architect, frontend specialists, backend developers, performance expert, ops engineer, designer, product manager...
How long would be the road from where you currently are, to this vision?

In a microservices setting, transversal teams are an anti-pattern, since they introduce, in their own way, coupling.
I am not saying that you should do away with all the transversal teams, as the organization can choose to retain common guidelines (e.g., architecture, modeling, management...) or capitalize on some technologies (e.g., orchestration or monitoring tools).
But to reduce the coupling, transversal teams should really act as communities of practice (rather than as a pool of resources to deliver features) and be located as close to the product teams as possible.

I will not go into the lengths of how hard change management is, but here goes for the compromise: how much energy into transforming the company's culture, and how much into delivery features to real users?

### Promise and compromise

At the end of the day, one should really wonder: what is there to win, and what am I ready to lose?

The only real guarantee of microservices architectures is to end up with a lot of small parts, lose transactional consistency, raise failure rates and challenge your organization. 

Decoupling, resilience, scaling, tech heterogeneity and small focused teams will not come for free; they can only be the result of a tremendous company-wide effort, from the leadership to the infrastructure teams and back, regarding people and their interactions, processes and tools.

On the one hand, only with all these options in hand will you be able to succeed in the challenges that come with modern visions and large-scale projects.
But on the other hand, without an impressive amount of upfront work and change in your organization, your microservices project will almost surely fail.

With a coupled model, a single unified tech stack, no resilience or scaling strategies and only transversal teams, you will get the worst of distributed systems, for no actual benefits.

Therefore, before actually jumping on the microservices bandwagon and losing the comfort of the monolith, be sure that you have what it takes to go all the way across the finish line.
In fact, if you are currently failing to build and maintain a monolithic architecture, what makes you think that you will do better with microservices? It's all the same, but way harder, in almost any imaginable way.

[![ROI along the road to microservices]({{ site.baseurl }}/public/resources/road-to-microservices/roi.svg)]({{ site.baseurl }}/public/resources/road-to-microservices/roi.svg)

Microservices only show you a road that was previously hidden. Walking on that road is all on you, and it will not be easy.

## Walking the road

So I hear that you know what you are doing, have quite some time and money in front of you, and decided to walk that road.

Now you need a map. 

For a variety of topics, we will discuss a spectrum of options of practices, tools and patterns, to compromise over.
They will range from large, coupled, but simple approaches, closer to what we find in monoliths, to the small, decoupled but more complex ones, pertaining to microservices approaches.

As we deep dive into it, do not lose focus on what we are trying to achieve: decoupling (**D**), resilience (**R**), scaling (**S**), tech heterogeneity (**H**) and small, autonomous, focused teams (**T**).
Let's grade the various patterns depending on their alignment with these 5 objectives: either aligned (✔️), misaligned (❌) or depending on implementation details or other factors (❔). 

### Architecture

How small the pieces we cut our monolith into?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Monolith | A single codebase with no further modularization | ❌ | ❌ | ❌ | ❌ | ❌ |
| Modular monolith | A single codebase split into modules with few dependencies (imports and method calls) between them | ✔️ | ❌ | ❌ | ❌ | ❔ |
| Large services | Multiple large services with each its own independent codebase (10-20 features or one DDD Bounded Context) | ✔️ | ❔ | ❔ | ❔ | ❔ |
| Large services | Multiple small services with each its own independent codebase (1-5 features or two weeks work) | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |

What does it mean for our domain model?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Global | A global graph of highly connected entities; a lot of foreign keys, no duplication | ❌ | | | | ❌ |
| Modular | Multiple smaller graphs of highly connected entities; foreign keys inside each graph, loose references across them, some duplication across modules | ❔ | | | | ❔ |
| Agregates | A lot of small DDD "Agregates" (1-10 classes each); only loose references across them, some duplication | ✔️ | | | | ✔️ |

### Technology stacks

What constraints are imposed on the choice of technologies (languages, frameworks, persistence...)?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Generic | One tech stack | | | ❌ | ❌ | ❔ |
| Standard | One standard tech stack; exceptions only when absolutely necessary | | | ❔ | ❔ | ❔ |
| Independent | Best stack chosen for each use case; probably with some guidelines | | | ✔️ | ✔️ | ✔️ |

### Integration

Do the different services share the persistence mechanism?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Shared | A shared schema in a single database | ❌ | ❔ | ❔ | ❌ | ❌ |
| Isolated | One schema per service in a single database | ✔️ | ❔ | ❔ | ❌ | ❔ |
| Independent | Best stack chosen for each use case; probably some common guidelines | ✔️ | ❔ | ❔ | ✔️ | ✔️ |

How do the different services communicate with each others?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| In-process | Method calls in the same process | ❌ | ✔️ | ❌ | ❌ | ❌ |
| Synchronous | Synchronous calls over the network, usually blocking requests to REST APIs | ❔ | ❔ | ✔️ | ✔️ | ✔️ |
| Asynchronous | Asynchronous, event-driven communication through a message bus | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |

### Failure handling

How do the services react to the failure of one of their dependencies? 

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| All-or-nothing | The services are either all up and running, or all failing at the same time | ❌ | ❌ | | | |
| None | No resilience strategy in place; failures cascade across services depending on each other | ❌ | ❌ | | | |
| Circuit breaker | Failing services are not called for some time; dependent services gracefully degrade their service in the meantime  | ❌ | ✔️ | | | |
| Bulkhead | Multiple identical instances behind an healthiness-aware load balancer; traffic is only routed to healthy instances  | ✔️ | ✔️ | | | |

### Consistency

What data consistency does the overall system provide? 

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Transactional | A shared database provides transactional consistency | ❌ | ✔️ | ❔ | ❌ | |
| Local | Transactions are only local to each microservice; for scenario spanning multiple services, the strategy is mostly based on hope and prayers | ✔️ | ❌ | ✔️ | ✔️ | |
| Transactional distributed | A shared component or pattern implements distributed transactions, usually 2-Phase Commit  | ❌ | ✔️ | ❌ | ❌ | |
| Eventual | A shared, resilient component, usually a message broker, is used to implement eventual consistency in the service layer | ✔️ | ❔ | ✔️ | ✔️ | |

### Build

How is the project versioned, built and packaged? 

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Monolithic | A single, large codebase in a single repository, all built and packaged as one | ❌ | | ❌ | ❌ | ❌ |
| Monorepo | Multiple services in a single repository, all built as one, but packaged independently | ❌ | | ❌ | ❔ | ❌ |
| Advanced monorepo | Multiple services in a single repository; caching mechanisms allow to only build and package changed services | ✔️ | | ✔️ | ❔ | ❔ |
| Multirepo | Each service its repository and build/packaging mechanism | ✔️ | | ✔️ | ✔️ | ✔️ |

### Deployment

In which format are the services packaged and deployed?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Platform-specific | The service is compiled and packaged in a format adapted to the target deployment platform (e.g., RPM package) | ❌ | | | ❌ | ❔ |
| Platform-agnostic | The service is compiled and packaged in a standard format, runnable on multiple deployment platforms (e.g., containers) | ✔️ | | | ✔️ | ✔️ |

Are the different services deployed independently?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| All-at-once | All the services are re-deployed everytime one service changes | ❌ | | ❌ | | |
| Train | All the services are re-deployed at a given interval | ❔ | | ✔️ | | |
| Independent | Any changed service is deployed immediately and independently; multiple versions of a same service can co-exist |✔️ | | ✔️ | | |

### Frontend

Is the frontend (UI) also split into modules or service? If so, what granularity?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Monolithic | A single, large frontend | ❌ | ❌ | ❌ | ❌ | ❌ |
| Shared UI lib | The codebase is also split into multiple codebase; a library of shared UI components is used | ❔ | ✔️ | ✔️ | ❌ | ❔ |
| Shared guidelines | The codebase is also split into multiple codebase; graphical guidelines are defined and shared | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |
| Micro frontends | Each microservice provides its own UI fragment; a global component assemble them at runtime | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |

### Authentication

How is authentication implemented?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Embedded | A simple login/password form, backed by a custom-made authentication mechanism | ❌ | ❌ | ❌ | ❌ | |
| SSO | A Single Sign-On mechanism is shared by all services, and integrated with each service independently | ❔ | ✔️ | ❌ | ✔️ | ✔️ |
| API Gateway | Authentication is handled by a shared API gateway; tokens are propagated to all services | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |

### Testing

What is the adopted testing strategy?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Hopeful | Testing is doubting, and a real developer never makes mistakes | ✔️ | ❌ | ✔️ | | ✔️ |
| Manual | Tests are mostly manual, usually made during test campaigns before important releases | ❌ | ❌ | ❌ | | ❌ |
| Mostly end-to-end | Most of the tests are at the UI or API level, and test the system as a whole | ❌ | ❌ | ❌ | | ❌ |
| Expert | A balance between a lot of unit tests, some integration tests, and a few end-to-end (smoke) tests | ✔️ | ❔ | ✔️ | | ✔️ |
| Sensei | Expert mode, plus consumer-driven contract testing between dependent services to avoid breaking changes | ✔️ | ✔️ | ✔️ | | ✔️ |

### Logging and monitoring

How are logging and monitoring implemented?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Embedded | Simple, custom-made implementations in each service, usually through librairies | ❌ | ❌ | ❌ | ❌ | |
| Standard | Despite custom implementations in each service, guidelines about format and exposition mechanism are shared | ✔️ | ❌ | ❌ | ✔️ | |
| External | Logging and monitoring concerns are externalized, usually delegated to the infrastructure layer (e.g., Kubernetes) | ✔️ | ✔️ | ✔️ | ✔️ | |

### Organization

How are teams organized around the codebase boundaries?

| Pattern | Description | D | R | S | H | T |
| --- | --- | :---: | :---: | :---: | :---: | :---: |
| Monolithic | A large, single team handles all the work | ❌ | | ❌ | ❌ | ❌ |
| Transversal | Teams are organized around arbitrary technical boundaries (e.g., frontend, backend and ops) | ❌ | | ❌ | ❔ | ❌ |
| Feature teams | Small, autonomous teams deliver end-to-end features, touching all parts of the codebase | ❔ | | ❔ | ❌ | ✔️ |
| Product teams | Small, autonomous teams are focused on their parts of the system (e.g. a DDD Bounded Context); internal open-source is used for shared components | ✔️ | | ✔️ | ✔️ | ✔️ |

### A map

So here you have it, the map of the road to microservices.

[![The road to microservices]({{ site.baseurl }}/public/resources/road-to-microservices/summary.svg)]({{ site.baseurl }}/public/resources/road-to-microservices/summary.svg)

To the left: large, coupled, but simple patterns, close to what is done in monoliths. To the right: small, decoupled but more complex ones, closer to microservice-friendly approaches.

The options marked with asterisks (*) are those corresponding to a reasonable implementation of a "Modular monolith".
As pictured in the previous section, they are a local optimum of the compromises on decoupling, resilience, scalability, technology heterogeneity and team organization, before having to pay the price of microservices architectures.
You would probably be surprised by how far a modular monolith, implemented with expertise, can scale.
We are talking hundreds of thousands of users, millions in revenue, hundreds of gigabytes of data, and tens of developers.

Marked with a dagger (†) are the options corresponding to the worst of both worlds, the feared "Distributed monolith", or microservices without a soul.
The dagger also represents what you will feel in your back now and then, in a very unpredictable and uncontrolled way if you stay in that zone for too long.
Quite surprising also is the size of the "valley of despair" between a modular monolith and an on-par microservices architecture.
We are now talking months of work, and unending trials and errors.

Of course, microservices can bring benefits that are out of reach of even the most perfect monolith. So if you really, really need them, now you know the road.
But before trying to reach this promised land, please double-check that you are correctly equipped, and absolutely prepared to go all the way. 

Do. Or do not. There is no try.

Or maybe there is.
Start small, with a monolith. Try to learn as much as possible about your domain, and sharpen your skills and tools (this will take months, if not years).
Once serving actual clients and propulsed by a successful business, investigate whether you really are limited by the vertical scalability limits and rigidity of your monolith, rather than by simple lack of skills and good practices (hint: you are not).
Then, check again that you are prepared for a long trip and that everyone, from developers to leadership, know the journey they embark on.

For the road to microservices is dark and full of terrors.
