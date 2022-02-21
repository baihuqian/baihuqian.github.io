---
layout: post
published: false
title: Distributed System Reading - Introduction
---
This is a new series of learning I took on to enhance my knowledge about distributed system. This series will focus on reading papers selected by [a University of Chicago reading course](http://uchicago-cs.github.io/cmsc23310/calendar.html).

Week 1 is the introduction.

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
