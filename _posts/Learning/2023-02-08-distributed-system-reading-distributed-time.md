---
layout: post
title: Distributed System Reading - Distributed Time
date: '2023-02-10 08:00'
tags:
  - Computer Science
---

This is a new series of learning I took on to enhance my knowledge about distributed system. This series will focus on reading papers selected by [a University of Chicago reading course](http://uchicago-cs.github.io/cmsc23310/calendar.html).

Week 2 focuses on distributed time. Lamport clocks and vector clocks are the core algorithms for distributed time topic.

## [Time, Clocks, and the Ordering of Events in a Distributed System](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/Time-Clocks-and-the-Ordering-of-Events-in-a-Distributed-System.pdf)

This is the famous Lamport clock paper.

In a distributed system, it is sometimes impossible to tell that one of the two events occurred first, without containing real clocks that are perfectly accurate. Each process in the distributed system can determine its own sequence of events. Communication in the system is via the form of message passing, and the receiving of a message is always after the sending of the message.

By introducing a logical clock that are monotonically increasing, such that (**Clock Condition**) if $$a$$ occurred before $$b$$ in a process, then $$C(a)<C(b)$$. Lamport clock can be implemented with two rules:

1. Each process increments its clock between any two successive events;
2. When sending a message (at event $$a$$), the message contains the timestamp of the sending process $$T=C_i(a)$$. Upon receiving this message, the receiving process set its clock to greater or equal to its present value $$C_j(b)$$ and greater than $$T$$.

These two rules define a total ordering of events. If $$a$$ is an event in process $$P$$ and $$b$$ is an event in process $$Q$$, then $$a$$ occurs before $$b$$ if $$C_P(a)<C_Q(b)$$, or $$C_P(a)=C_Q(b)$$ and $$P<Q$$ (an arbitrary but total ordering of processes as tie-breaker). The ordering is not unique and depends on the choice of the clocks. The determination of ordering can be performed locally by each process without centralized process or physical clock.

## [Timestamps in Message-Passing Systems that Preserve the Partial Ordering](https://fileadmin.cs.lth.se/cs/Personal/Amr_Ergawy/dist-algos-papers/4.pdf)

This paper introduces vector clock. Lamport clock defines a possible total ordering, but vector clock defines all possible partial orderings. The key is that communication events from "boundaries" that limit the possible interleavings of concurrent events. We can infer the ordering from the information of when each process last communicated with every other process.

In asynchronous communication, timestamps are represented as an array (vector): $$[c_1, c_2, ..., c_n]$$, each value is an integer clock for every process in the network. $$e_p$$ is an event in process $$p$$, and $$T_{e_p}$$ is the timestamp of $$e_p$$. Timestamps are maintained by:

1. initialized to all zero.
2. the local process $$q$$'s clock is incremented at least once for every event.
3. the entire array is attached to every communication.
4. upon receiving a message $$e$$, the sender process $$p$$'s clock is set to $$T_{e_p}+1$$ (one plus the receiving timestamp) if $$T_{e_q}\leq T_{e_p}$$. Every other process's clock is set to $$max(T_{e_p},T_{e_q})$$.
5. timestamps are never decremented.

The special handling for the sender process is required because the communication between two processes are not always in order.

To compare two events $$e_p$$ in process $$p$$ and $$f_q$$ in process $$q$$: $$e_p$$ is before $$f_q$$ iff $$T_{e_p}(p)\lt T_{f_q}(p)$$, or iff $$q$$ has received a signal from $$p$$, either directly or indirectly, that was sent after, or at the same time as the exeuction of $$e_p$$.

Synchronous communication can be thought as two consecutive asynchronous communications where two processes *exchange* the timestamp., and the timestamp update rule (#4) can be simplified without the special handling for the sender process (i.e., each value is set to $$max(T_{e_p},T_{e_q})$$ for message exchange between $$p$$ and $$q$$ at event $$e$$).

To compare two events $$e_p$$ in process $$p$$ and $$f_q$$ in process $$q$$: $$e_p$$ is before $$f_q$$ iff $$T_{e_p}(p)\leq T_{f_q}(p)\land T_{e_p}(q)\lt T_{f_q}(q)$$. The first half of the conjunction asserts
that process $$q$$ has received a clock value from process $$p$$ at least as recent as the execution of event $$e_p$$. If this is the case then $$f_q$$ must have been executed after $$e_p$$. The comparator is $$\leq$$ rather than $$\lt$$ to allow for the possibility that $$e_p$$ was itself a communication event (in which case $$q$$ may have up-to-date information about $$p$$ when $$f_q$$ was executed). The second half of the conjunction simply states that process $$p$$ does not have up-to-date information about process $$q$$, i.e. $$e_p$$ is not the same event as $$f_p$$.

When the list of processes can dynamically change, the timestamp arrays can be extendable lists. Each process adds a slot for a newly created process to the array when, and if, it receives any communication (either directly or indirectly) from that process. VVhen comparing timestamps, if no entry exists for one of the processes involved we simply substitute zero.

## [Virtual Time and Global States of Distributed Systems](http://www.vs.inf.ethz.ch/publ/papers/VirtTimeGlobStates.pdf)

This paper is a longer version of vector time, and how to use vector time to gain global states.
