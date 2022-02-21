---
layout: post
published: false
title: Distributed System Reading - Introduction
---
This is a new series of learning I took on to enhance my knowledge about distributed system. This series will focus on reading papers selected by [a University of Chicago reading course](http://uchicago-cs.github.io/cmsc23310/calendar.html).

Week 1 is the introduction.

## [A Note on Distributed Computing](https://www.cc.gatech.edu/classes/AY2010/cs4210_fall/papers/smli_tr-94-29.pdf)
This paper argues why the unified object model fails and why engineers must be aware of differences between local and distributed computing.

In unified object model, an object, whether local or remote, is defined in terms of a set of interfaces declared in an interface definition language. The vision is that developers write their applications so that the objects within the application are joined using the same programmatic glue as objects between applications, but it does not require that the two kinds of glue be implemented the same way. This vision is centered around the following principles, and all of these are false:

* there is a single natural object-oriented design for a given application, regardless of the context in which that application will be deployed;
* failure and performance issues are tied to the imple- mentation of the components of an application, and consideration of these issues should be left out of an initial design; and
* the interface of an object is independent of the context in which that object is used.

The paper aruges that unified object model is missing the point. The hard problems in distributed computing are not the problems of how to get things on and off the wire. The hard problems in distributed computing concern dealing with partial failure and the lack of a central resource man- ager. The hard problems in distributed computing concern insuring adequate performance and dealing with problems of concurrency. The hard problems have to do with differences in memory access paradigms between local and distributed entities.

The major differences between local and distributed com- puting concern the areas of latency, memory access, partial failure, and concurrency. The difference in latency is the most obvious, but in many ways is the least fundamental. The often overlooked differences concerning memory access, partial failure, and concurrency are far more difficult to explain away and *the differences concerning partial failure and concurrency make unifying the local and remote computing models impossible without making unacceptable compromises*.

Partial failure is a central reality of distributed computing. In the case of local computing, failures are either total, affecting all of the entities that are working together in an application, or detectable by some central resource allocator (such as the operating system on the local machine). This is not the case in distributed computing. Not only is the failure of the distributed components independent, but there is no common agent that is able to determine what component has failed and inform the other components of that failure, no global state that can be examined that allows determination of exactly what error has occurred. In a distributed system, the failure of a network link is indistinguishable from the failure of a processor on the other side of that link. A central problem in distributed computing is insuring that the state of the whole system is consistent after such a failure, such problem doesn't exist in local computing. Partial failure requires that programs deal with indeterminacy.

Distributed objects by their nature must handle concurrent method invocations. Multi-threaded application has not real source of indeterminacy of invocations of operations. A non-distributed system, even when multi-threaded, is layered on top of a single operating system that can aid the communication between objects and can be used to determine and aid in synchronization and in the recovery of failure. A distributed system, on the other hand, has no single point of resource allocation, synchronization, or failure recovery, and thus is conceptually very different.

The lack of robustness of distributed system is not in the implementation; issues that can’t be answered by supplying a “more robust implementation” because the lack of robustness is inherent in the interface and not something that can be changed by altering the implementation.

Therefore, differences in latency, memory access, partial failure, and concurrency make merging of the computational models of local and distributed computing both unwise to attempt and unable to succeed. Rather than trying to merge local and remote objects, engineers need to be constantly reminded of the differences between the two, and know when it is appropriate to use each kind of object. Distributed objects are different from local objects, and keeping that difference visible will keep the programmer from forgetting the difference and making mistakes.

## [Fallacies of Distributed Computing Explained](https://arnon.me/wp-content/uploads/Files/fallacies.pdf)
There are eight fallacies of distributed computing and they have remained the same since the inception of distributed computing.

#### The network is reliable.
Every device has a MTBF.

Think about hardware and software redundancy and weigh the risks of failure versus the required investment.

Bake handling of lost messages/calls into the software.

#### Latency is zero.
Latency is how much time it takes for data to move from one place to another (versus bandwidth which is how much data we can transfer during that time).

While bandwidth has increased by a lot over the years, latency has not. Latency is limited by physics.

Think about the latency of both blocking (synchronous) operations and non-blocking (async) operations to your application.

#### Bandwidth is infinite.
Bandwidth is not only related to the line rate. TCP bandwidth is determined by latency (rtt or round-trip time), packet loss, and packet size (MTU). For example, New York to Los Angeles. Round Trip Time (rtt) is about 40 msec, and let's say packet loss is 0.1% (0.001). With an MTU of 1500 bytes (MSS of 1460), TCP throughput will have an upper bound of about 6.5 Mbps. With 9000 byte frames, TCP throughput could reach about 40 Mbps.

#### The network is secure.
The implication is we need to build security into the applications from Day 1.

#### Topology doesn't change.
Network topology is changing **constantly** due to changing physical resources, updating authentication methods, moving devices, and faulty devices, etc.

Not depend on specific endpoints or routes. Provide location transparency (multicast) or discovery services (Active Directory, JNDI).

Another strategy is to abstract the physical structure of the network. The most obvious example for this is DNS names instead of IP addresses.

#### There is one administrator.
There are many different people, teams, and companies that your software is related. Due to distributed nature, software upgrades mean your software must work with multiple versions of other components.

You need to remember that administrators can constrain your options (administrators that sets disk quotas, limited privileges, limited ports and protocols and so on), and that you need to help them manage your applications (with tooling).

#### Transport cost is zero.
Going from application level to transport level is not free due to marshalling (serialize information into bits).

The costs (as in cash money) for setting and running the network are not free. The amount of network capacity required by your application can be huge and thus expensive.

#### The network is homogeneous.
The network is not homogeneous at the physical layer.

It is worthwhile to pay attention to the fact the network is not homogeneous at the application level. The implication of this is that you have to assume interoperability will be needed sooner or later and be ready to support it from day one (or at least design where you'd add it later).

Do not rely on proprietary protocols.
