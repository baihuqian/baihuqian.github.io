---
layout: "post"
title: "Eager Loading vs. Lazy Loading in High-Performance Software-Defined Networking"
date: "2019-03-25 12:43"
---

This post aims to outline how to efficiently distribute data among high-performance software-defined networking (SDN) service at scale.

## Eager Loading vs. Lazy Loading
There are mainly two ways of fetching or loading data across a distributed system: eager loading and lazy loading.

Eager loading is to have the system load all the data and keep it in sync with the source of truth (or the system itself is the source of truth), independent from the need of the data consumer. Note that it is orthogonal to the data distribution model, i.e., "pull" (the system periodically fetches the data from the source of truth) or "push" (another system sends incremental or full updates to this system) model.

Lazy loading is to have the system load only the necessary data to fulfill a request when the request arrives. In other words, it is a just-in-time fulfillment of data requirement for incoming requests.

There are no clear advantages for one over the other; often the decision of which loading mechanism is chosen depends on the characteristics of the software system. For example, an API service backed by a database is a classic lazy-loading scheme. But, if a caching middleware is used to improve the performance, the cache itself can be either lazy-loading or eager-loading, depend on how data are cached. Thus, to analyze the pros and cons of the data distribution mechanism, let us first examine how a networking service is organized.

## Architecture in Hardware Networking Devices: an Oversimplified Overview
Traditionally, networking is made possible by various hardware devices, such as routers, switches, and firewalls, designed and manufactured by vendors such as Cisco, Juniper, Palo Alto Networks, and Huawei. These gears are purpose-built to perform a set of network functionalities very well, such as routing, switching, packet inspection.

On the other hand, almost all network functionalities have been implemented as software, either as a part of the Operating System or kernel module, or an application. However, the performance of software-based implementation lags far behind from their hardware counterparts with the same amount of computing power. Why so?

Networking functionalities can be roughly divided into two categories: packet handling and configuration. Packet handling includes forwarding (for routing and switching), encryption/decryption, encapsulation/decapsulation, packet inspection, and handling of control packets such as ICMP or BGP. Configuration provides an interface, such as a Command-Line Interface or web Console, that allows network administrators to set up, configure, monitor, and troubleshoot the network. They are often referred to as the data plane and the control plane, respectively.

Packet handling operates under a fixed and finite set of rules, requires high throughput, and often involves low-level operations on memory or network buffer. Packet handling rules are well-defined in RFCs, and changes are prolonged (one example would be the migration to IPv6).

Configuration, on the other hand, is much slower in speed but much faster in changes. It involves high-level operations suitable for human interactions, but functionalities evolve through time with firmware upgrades.

The secret of high-performance networking provided by hardware devices is to tackle packet handling and configuration in vastly different ways. Packet handling is done in purposely-built chips and circuit boards called line cards, while the configuration is provided by a custom OS running on a CPU, often much cheaper than a general-purpose computer. The device configuration is written to the onboard memory by the device OS in a specialized format.

Compared to software-based networking solutions, the CPU of the networking devices is freed from doing any of the mundane packet handling routines, thus requires a lot less computing capacity. However, whether the configuration and the interface of the device can be easily done and upgraded, the specification of the line card dictates the network functionality and capacity of the device that cannot be easily upgraded unless a new one is installed.

On the other hand, because configuration changes are thought to be rare and the total amount of configurations is not significant in most cases, device vendors often design the weakest configuration capacity possible to reduce cost. Thus, the hardware devices are often described as "strong data plane, weak control plane." In most cases, upon a commit, the entire configuration for the device is generated and written to the line cards.

## An Architecture for High-Performance SDN
This post is not a deep dive on the architecture of a high-performance SDN system. But I will cover the basics here to facilitate the discussion.

The goal of a high-performance SDN system is to achieve similar performance than the hardware counterparts and much larger scale and more flexibility in configuration.

For the packet handling part, we should design a system dedicated to a set of functionalities that runs on a horizontally scalable fleet. For the configuration part, we should provide an API interface that provides rich functionalities at scale. Therefore, the gap between the two parts are much bigger, noticeably:
1. Packet handling and configuration should speak different "languages" on the same set of configuration data plus their own metadata, to achieve both high performance and usability.
2. Configuration is naturally distributed, and no single host can manage all the data in its entirety.
3. Decoupling packet handling and configuration increase agility as any interface change no longer require synchronous deployments on both sides.

Therefore, a layered approach can help to bridge the gap. First, a layer or even layers of purposely-built data plane components can cope with the increasing complexity of configurations while maintaining high-performance compatibility with the core packet handling layer. This layer can act as the middleware and cache for the fast path (packet handling) and the slow path (configuration). Then, a layer of configuration distribution services, or the "distribution plane," efficiently gather, partition, and distribute configurations handled by the control plane.

## Data Distribution
Finally, we can discuss how to distribute data to the data plane efficiently.

Due to the high-performance requirement of the packet handling layer, in most cases, if not all, specialized data structures occupying a large portion of the host memory is used. For example, fixed-size hash tables, placed into huge pages, are used to store per-flow information at Layer 4; trie is used to store route tables to perform Longest-Prefix Match (LPM) at constant time. These tables are pre-allocated with a fixed size to reduce memory allocation overhead and are limited to the capacity of the host. To reduce cost, host types with the largest network performance are chosen, resulting in limited memory space. Therefore, configurations used to fulfill packet handling are lazily loaded with a pre-defined eviction scheme. This ensures the configurations required for each flow exists on the host for the most part of the traffic flow while supporting a large number of flows with dynamically changing configurations. The drawbacks are:
1. the local caching adds some latency to the distribution of configurations;
2. efficient eviction logic and handling of cache miss must be implemented as part of the packet handling logic, increasing the complexity;
3. a purposely-built layer of data plane services must be built to accompany it and support efficient configuration retrieval;
4. Jitter is experienced in traffic flows going through such service. Therefore, the supporting layer must be implemented as effectively as possible.

The supporting layer or layers, act as multiple cache layers depend on the complexity of the configuration and the tolerance of jitters introduced, can either implement lazy loading or eager loading, or a mixture of both. The topmost supporting layer (the farthest layer from the packet handling layer) must implement eager loading, as the latency is too large to query the source of truth directly. The middle layers can perform some caching of lazily-loaded data, or eagerly retrieve data.

The most straightforward design is to have a two-layer data plane: the packet handling layer and a configuration-caching layer. The packet handling layer queries the configuration-caching layer using special-purpose network protocols as lazy loading; the configuration-caching layer eagerly loads all or portion of the configurations, usually partitioned along customers, extracts relevant data, and transforms into efficient data structures. This way, the packet handling layer can be implemented with minimum state and highest efficiency, with a focus on networking performance; the configuration-caching layer can tackle scaling challenges through partitioning, specialized data structure, and a push model.
