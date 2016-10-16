# Ops Defined Network
Ops Defined Network (ODN, pronounced */o-den/*) is a library and toolkit for building adaptive distributed services. If you are familiar with Netflix's [Hystrix project](https://github.com/Netflix/Hystrix), then all you need to know is ODN was originally pitched as "Hystrix on steroids." Otherwise, this document aims to explain everything you need to know about ODN: when, why, and how to use it with your applications.

## What is an Ops Defined Network?
I made it up. An ops defined network is a network where the applications, their relationships, and their behaviors dictate the shape and behavior of the entire network. ODN is similar to a Software Defined Network (SDN), but instead of manipulating the physical routers and switches of a network ODNs control and manipulate at the application and service level of a network.

## When to use ODN?
ODN is not for everyone. Very few organizations have a distributed service infrastructure large enough to justify the use of an ODN. A large distributed service architecture suitable for ODN has tens to hundreds of interconnected (micro-)services, most of which require multiple machines running in multiple data centers for resiliency. If this does not sound like you, please do not use ODN. Of course, think of ODN when the time does hopefully come.

## Why to use ODN?
Here is a sampling of the potential functions offered by ODNs.

### Feature toggles and remote configuration
Production systems which continually add new features will initially place new features behind one or many feature toggles which allow these features to be turned on and off at whim, remotely and anytime in the future. These toggles effect service behavior. ODN provides a broader mechanism for feature toggles called remote configuration: passing parameters to relevant services which modify their behaviors. These parameters can be anything, not just boolean flags, and can be changed in real-time based on not just the single service but the entire network state.

### Scaling for Service Level Agreements
A Service Level Agreement (SLA) is contract between a service provider and the customer using the service which defines the guarantees of the system that the provider must maintain in order to avoid the consequence stipulated in the contract. Extensive monitoring of up-times, performance, throughput, and other such metrics are needed to analyze whether a SLA is being upheld and if it isn't it is a huge problem. ODN, given minimums and maximums of each metric defined in an SLA, can automatically evaluate whether the system is in compliance and, if not, actively scale the relevant services to meet expectations. This includes reconciling multiple SLAs for the same services, the addition of new SLAs, and removal of expired SLAs.

### Cascading failure adaption
If a service has multiple dependencies, those dependencies may also have their own dependencies, and so on forming large trees and chains that are characteristic of almost any distributed services architecture. Currently, tools like Hystrix can help us deal with dependency chain failures using a circuit-breaker construct where each link in the chain eventually discovers its dependency's failure and automatically fails itself. ODN lets us do much better: instead of each service having to discover things about its own dependencies, a failed dependency emits a failure to ODN and ODN can put every service along the relevant chains into proper failure modes in real-time with no trial-and-error work.
