---
layout: post
title: "The road to microservices"
date: 2021-02-28
categories: architecture
tags: english
---
Tech conferences piling up presentations from large cloud-native companies, shiny new frameworks, consulting firms building up their portfolios, software engineers discussing how to ride the hype at the coffee machine... Microservices are everywhere.
A lot has already been said and written about them, often making them look like a silver bullet.
But why leave the comfort of the well-known (sometimes layered, sometimes modularized) monolithic architecture?
Do you really need what microservices offer? Are you really ready to lose what microservices sacrifice?
This article tries to summarize my view on the "microservices compromise", what it requires and what it implies, beyond splitting a codebase into small pieces.  

For the road to microservices is dark and full of terrors.

[![ROI along the road to microservices]({{ site.baseurl }}/public/resources/road-to-microservices/roi.svg)]({{ site.baseurl }}/public/resources/road-to-microservices/roi.svg)

<!--more-->

## The promise

On paper, microservices architectures offer a lot of benefits.

### Small, focused and decoupled services

It is known that small systems are easier to reason about and build. Who has never noticed that software is never produced more efficiently than during the first few weeks of development?
This also holds for maintenance: regardless of team seniority and turnover, it is always easier to keep alive and change behavior in a small codebase than in a large bowl of spaghetti code.
Eventually, all software is retired, either because too many mistakes were made and the team decides to start over, or because the requirements or the environment changed too much. 
Again, replacing multiple small services is easier than redesigning an entire software, but the main benefit comes from the fact that it can happen progressively, over the cours of multiple months or years.
No more multi-year projects to completely overhaul entire codebases!

Good news! Microservices architectures are all about splitting software in multiple services and keeping them small.

When done right, this code split follows the lines of the various concerns handled by the application.
This means that each service is focused on a small, specific set of features, and does not have to care too much about the rest of the application logic.
The benefits of such a decoupled architecture are obvious: ease of development and evolution, fewer dependencies between teams and roadmaps, fewer bugs and anomalies... 

### Technology heterogeneity

In a monolithic architecture, the choice of language and framework is made once, usually at the very beginning of the project.
Librairies tend to accumulate as new requirements arrive or because of team turnover and personal developer preferences.
This is why all monolithic architectures usually go for a large, generic framework (e.g., Spring for Java codebases) and a standard storage solution (e.g., a relational database).

*"Jack of all trades, master of none"*, as the saying goes. Despite all their (often impressive) efforts, how could such frameworks provide, at the same time, the optimal solution for use cases requiring a fast startup (async tasks), best performance during a few minutes or hours (batch jobs) and everything else (high frequency/low complexity and low frequency/high complexity) in the spectrum of today's web applications?
How could a database provide, at the same time, the best options to store structured, unstructured and binary data, regardless of response time, volume, scaling and consistency requirements?

The list does not stop there, and soon enough a new, modern framework will appear that looks better suited for the job at hand.
At this point, there is no good option: should you stick with the old, deprecated framework, or rewrite the entire application? You are stuck.  

In microservices architecture, the rules are different.
You can choose the right stack (language, frameworks, librairies, storage...), at the right moment, on a use case basis.
This could mean using, at the same time, a compiled language with no framework (e.g. Go) for services for which startup time is crucial, a lightweight web framework with an efficient, distributed cache for nd a different web framework (e.g. Node, Express and Redis) for high frequency/low complexity workloads, and an advanced framework in a typed, interpreted language (e.g. Java and Spring) for low frequency/high complexity use cases.

### Resilience and scalability

Monoliths offer what I call "all-or-nothing" availability. Taking aside a few uncommon edge cases, the application is either up and running, or not.
This situation is quite easy to reason about, since the resilience of the system boils down to your ability to keep the monolith process running.
If anything goes wrong (host failure, crash, network failure, insufficient disk space...), the whole service is gone.

With microservices, a failure only affects one or a few of the multiple parts of the system, leaving most of the services running.
This means that you now have the option to react on failure and expose a degraded service to your users.

Regarding scalability, monoliths can only grow vertically.
That is, the only way to make a monolith faster or handle more requests is to make it run on a bigger, faster host.

Microservices, on the other hand, open the door to multiple scaling strategies:
- vertical scaling: of course, why not add more RAM and CPU to make things faster?
- horizontal scaling: the load is shared between multiple identical instances behind a load balancer
- sharding: the load is split between multiple instances depending on the data it needs to access
- functional decomposition: the service is split into smaller services with technologies and scaling strategies specific for each kind of load
Of course, all scaling strategies are not always available. For instance, horizontal scaling requires the service to be stateless, and sharding requires the computation to be localized enough for the underlying data to be sharded. 
One could also argue that some of these scaling strategies is also possible with monoliths, but to me this is already a step on the road to microservices!
  
The essence of it all is that, in microservices architectures, the scaling strategy can be chosen independently for each service: a batch microservice could scale vertically, while a web server is made to scale horizontally.
Independently, transactional use cases can be handled by an object model backed by a document-oriented database, while analytics use cases are moved to another microservice using a time series database.
None of these options will ever be available in a monolithic architecture.

### Teams and organization

As the size of a software grows, the size of the corresponding team tends to grow accordingly, often with the incorrect assumption that more people means faster delivery.
It is well documented that the effort required to synchronize team members is not linear in the number of people, but rather quadratic: to synchronize 2 people, 1 phone call is enough ; to synchronize 5 people, 20 phone calls are required.
Large meetings only solve this issue up to a point, arguably around 5 to 10 people.

Microservices offer the opportunity to naturally split the work and knowledge between small teams, each focused and knowledgeable about their microservices and the few they interact with.
Although synchronization and collaboration between teams is still required, the frequency and length of such events should be dramatically reduced.


## The untold story of the compromise

After reading the previous section, you now really want to use microservices in your current project and probably all the other ones, past and future!
But as everybody knows, there is no silver bullet, or no free lunch. Only compromises.
So let me sing you a song that is rarely sung, the one about the road to microservices, and everything you lose along the way!

### Small and coupled

Models in monoliths are not created as a large pile of coupled objects by design.
They progressively become so, because concepts in the real world are inherently coupled, and simplifying reality to extract and maintain a decoupled model over time is no easy task (it can actually be a full-time job).
In monoliths, coupling can stay unnoticed for quite some time before the first symptoms appear, affecting either development velocity, ease of maintenance or performance.
In microservices, there is nowhere to hide. With parts of the model distributed across the network, the cost of coupling is orders of magnitude higher, as it will almost instantly result in maintenance burden and performance bottlenecks.
A lot more effort will also be required to fix the model: instead of refactoring a local set of objects, multiple services will now need to be merged, split and rearranged, with a global impact on the architecture.

Do not expect decoupling to come for free from splitting a large codebase into small pieces.
In fact, it works the other way around: the split into small pieces will only be made possible by your team's ability to design a decoupled model to begin with.
If your monolith looks like a bowl of spaghettis, it is a strong hint that you lack the modeling skills required to succeed in microservices.

With microservices, the model now has boundaries that are hard to move, and the possibility of a large scale redesign is essentially lost.
If you get these boundaries wrong, you end up with a lot of small, coupled services, which is absolutely worse than a large, coupled model in a monolith.

The compromise can therefore be stated as follows: how confident are you in your ability to extract and maintain a useful, decoupled model? How much splitting into small pieces are you willing to bet on it?


### The technology zoo

Like for everything else, technological flexibility is great, but it comes with a price.
Instead of one technology stack to master, you now have two, three, five, ten of them, each possibly made of multiple languages, frameworks, librairies and tools!
We are not only talking about development skills, but rather about a company-wide matter, from development and testing to integration, build tooling, deployment, configuration, monitoring, performance management, security...
Having multiple technologies in production means having to maintain a pool of experts for each of them, in each department. Otherwise, the next rewrite from scratch will not be because of code quality but because of a lack of skills!

Letting everyone use the languages and tools they like and hope for the best is therefore not a valid strategy, and some guidelines must be established.
They can take the form of a reduced set of technology stacks to choose from (e.g., Netflix seems to focus on JVM technologies), or a particular process to test, validate and train people on new languages or frameworks. 

Too many constraints and you don't get the benefits of technology heterogeneity listed in the previous section; too few constraints and everybody has fun at the technology zoo!
Quite obviously, this is again a matter of compromise.


### Cascading failures and stateful services

Compared to monoliths and their all-or-nothing availability, microservices bring a different equation to the table.
Because they are composed of more moving parts (multiple hosts, multiples services, multiple network links...), they are statistically more likely to fail.
Pushed to a certain scale, a microservice architecture is, at any point in time, statistically more likely to have at least one failing component than to be entirely healthy.
Seriously, do the math. Assume a 99% overall uptime for all your components:
- with only one component, the probability to be healthy is, well... 99%
- with *N* components, the probability to be healthy is 99%<sup>N</sup>
- 70 components are enough to have a <50% chance to not have any failure at a given time!

So, microservices actually bring more failure, not less!
But now you have the option to keep them localized and compensate for them.
Again, this does not come for free, and there is actual work to be done to get there!

The first solution that comes to mind is the *Bulkhead* pattern, in which multiple identical instances are used behind a load balancer to handle traffic.
If one of the instances crashes, the load balancer stops sending traffic to it until it recovers (in some advanced versions of this pattern, the failing instance is automatically killed, and another one is spawned somewhere else).
Beyond the infrastructure and configuration work implied here, there is also a hidden requirement that the service handling traffic is stateless.
Otherwise, how could the load balancer transparently redirect traffic from a failing instance to a healthy one?
This means that if your services are not stateless yet, you also need to invest to transform them accordingly... 
  
In the end, do you prefer investing on such error handling patterns to fight against statistics, or reducing the number of moving parts? Who said "compromise"?


### Transversal teams and change management

Code is easier to change than organizations or culture. And cutting the code into small pieces will not magically change the way people work together.
If your entire enterprise is organized around technical boundaries (e.g. a frontend team, a backend team, and an Ops team), massive efforts will be required to evolve to a lot of small, focused, multidisciplinary teams.
Imagine, 10 teams with each their architect, frontend specialists, backend developers, performance expert, ops engineer, designer, product manager...
How long would be the road from where you are to this vision?

In a microservices setting, transversal teams are an anti-pattern, since they introduce, in their own way, coupling.
I am not saying that you should do away with all the transversal teams, as the organization can choose to retain common guidelines (e.g., architecture, modeling, management...) or capitalize on some technologies (e.g., orchestration or monitoring tools).
But to reduce the coupling, transversal teams should really act as communities of practice rather than as a pool of resources to deliver features, and be located as close as the product teams as possible.

I will not go into the lengths of how hard change management is, but here goes for the compromise: how much energy into transforming the company's culture, and how much into delivery features to real users?


### Promise and compromise

At the end of the day, one should really ask: what is there to win, and what am I ready to lose?

Ending up with a lot of small services is the real only guarantee of microservices architecture.
Decoupling, resilience, scaling, tech heterogeneity and autonomous focused teams will not come for free.
They are the result of a tremendous company-wide effort, from the leadership to the infrastructure teams, affecting people, process and tools.
And without them, your microservices architecture will quite surely fail: with a coupled model, unified tech stacks, no resilience or scaling strategy and only transversal teams, you will get the worst of distributed system, for no concret benefits.

In fact, if you cannot successfully build and maintain a monolithic architecture, what makes you think that you will do better with microservices?
It's the same, but way harder, in almost any imaginable way.

Microservices only show you a road that was previously hidden. Walking on that road is all on you, and it will not be easy.



## This is no straight road



Start small, from the monolith to a modular monolith, multiple years
Have the gas to cross the valley?

[![The road to microservices]({{ site.baseurl }}/public/resources/road-to-microservices/summary.svg)]({{ site.baseurl }}/public/resources/road-to-microservices/summary.svg)
