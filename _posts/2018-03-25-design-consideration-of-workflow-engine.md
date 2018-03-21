---
layout: post
title: Design Consideration of Workflow Engine
date: '2018-03-25 15:05'
tags: Design
---

In a distributed, service-oriented software system, it is often required to achieve some goals with a combination of one or more API calls, some database transactions, or others. These operations are remote to the software system, thus it is possible to encounter failure, network packet loss and latency, or the host running the software can fail. Thus, a good workflow engine design is vital to the correctness and the performance of the software system.

In this post, I will attempt to lay out the design considerations of a good workflow engine.

# Workflow Modeling
So, what is a workflow?

There are formal definitions for a workflow. But in my understanding, a workflow is a finite, ordered series of actions that transform the start state to the end state. It can be modeled as a directed acyclic graph with one or multiple paths between the initial state and end state. Each state is a node and each action is an edge. For each state, there can be multiple actions given some conditions, but only one action can be taken on a state, and the choice of action is well-defined. The initial and end state of a workflow is well defined, and the change set of each action is well-defined. Therefore, each workflow state is well-defined.

The state of a workflow is the state of a collection of resources, database entries, or states in remote systems. An action in a workflow is a mutation API call, a database transaction, etc. that changes at least one aspect of the state of the workflow. Non-mutating APIs, read-only database accesses are not actions.

# Design Challenges
Given the distributed nature of the software system, the workflow engines faces a series of challenges:

1. The state of a workflow is distributed, and is expensive to construct from the source of truth.
2. Actions in a workflow are not always reliable. Because they go through networks, the request can be lost, the remote system can be unresponsive, the response can be lost, or the latency can be too long. While it is quite unlikely that a successful action does not result in a correct transition of states, a failed action often produces various changes to the state, including NO-OP (action is not taken), expected operation (action is taken but response is lost), and sometimes unexpected operation.
3. The workflow engine can fail, so in-flight workflows must be picked up cleanly by another host.
4. If more than one host runs the workflow engine, workflows must be efficiently and correctly distributed to them to ensure one and only one host works on any workflows at any time.
4. Actions are rate-limited by type. Proper rate limiting must be done to protect workflow engine's dependencies.
5. Some actions are asynchronous and state transitions of such actions take time. The workflow engine must timely and efficiently discover state transitions.

# Design Principles
To address aforementioned challenges, the following design principles should be considered:

## Action
1. Each action should include only one mutating operation that, under normal circumstances, can either succeed as a whole, or fail as a whole. This includes one API call, one database transaction, etc. Multiple operations can be grouped into one action if transactional lock can be used to rollback partially succeeded operations when latter operations in the same action fail.
2. After an action is taken successfully, the change set of the workflow state is well-defined. There should be no ambiguity of the change an action can apply.
4. The failure mode of an action is finite and well-understood.
5. Any API calls, database operations, etc. should be rate-limited.
5. Result of asynchronous actions should be retrieved timely and efficiently.
6. Actions should be idempotent whenever possible. If an action cannot be idempotent, it can either be taken multiple times without side effect, or the consequence of the extra calls does not impact the workflow state and can be efficiently cleaned up independently.

## Workflow State
1. Workflow state should be cached in memory after an action is taken, and the next action can be taken without reconstructing the workflow state from source of truth.
2. Workflow state should be persisted in data store after each action is taken, so that when it is assigned to a different host, its state can be efficiently constructed without consulting the source of truth.
3. If different actions can be taken based on external conditions, these conditions are well-defined and their true state can be efficiently discovered.

# Design
A workflow engine can be divided into the following functional units.

## Workflow Worker
There are three paradigms of designing workflow workers. First, each worker works on only one workflow item, until this item completes. This is called "dedicated worker" paradigm in this post. Second, each worker works on only small set of actions, and workflow items are worked by multiple workers in a defined order. This is called "assembly line" paradigm. Third, workers are agnostic to the workflow item or the specific action; they receive the workflow state and the action, execute the action and record the state change, and move on to the next one, which can be the same workflow item, a different workflow item in a different state, or even a different type of workflow. Because the workers are completely dumb, this is called "dumb worker" paradigm.

There are both advantages and disadvantages in all paradigms. In the dedicated worker paradigm, workflow state is maintained by a single worker, eliminating the need of passing state from one worker to the next. All the actions of this workflow must be defined in a single place, which can be good or bad depending on the situation. However, this process does not allow batch processing, and thus a workflow scheduler that distributes workflows to workers is required.

In the assembly line paradigm, workflow actions are spread to multiple workers, which can reduce readability of the code. It also requires passing workflow state between workers. However, batch processing is possible.

In the dumb worker paradigm, workers are dead simple. However, workflow management must be done carefully. The state of all in-progress workflows must be tracked carefully, so that no workflows are left unworked or state gets stale. The workflows must be distributed to workers according to carefully-calibrated policies, or the duration of workflows can be indeterministic based on the load in the system.

I recommend to model complicated but less frequent workflows with the dedicated worker paradigm, and define large number of simple workflows using the assembly line paradigm.

## Workflow Scheduler
To ensure workflows are organized in a desirable way, whether to ensure fairness between workflow items or to prioritize in-flight items over new items, or to distribute workflow items to multiple hosts running the workflow engine to avoid contention, a workflow scheduler is helpful to manage workers so that workflows can be processed in the desired way.

In the assembly line paradigm, back pressure must be implemented by the scheduler so that the speed of each worker is limited to the slowest one, to ensure no accumulation of workflow items at the bottleneck. In the dedicated worker paradigm, the number of concurrent workers should be limited so that no dependency is overwhelmed.

In the dumb worker paradigm, scheduler is a must. I will not dive into how the scheduler should be implemented, because we can get inspiration from the OS scheduler. If you think a workflow item as a thread, its state is the thread stack, and its next action is the combination of program counter and the machine instruction, then you can almost derive a design from various OS schedulers. However, given the complexity of OS scheduler and the often-distributed nature of the workflow engine, I don't recommend this model.

## Rate Limiter
There is always a limit of capacity for any dependencies. This is often defined as allowed Transaction per Second, or TPS, from a given service. Thus, to efficiently use the CPU and network bandwidth, the workflow engine must implement rate limiter at the client side.

The implementation of Rate Limiter depends on the design of the worker. If workers are designed as threads, then the threads must be paused when allowed TPS is exceeded. If workers are designed as event handlers, API calls or database transactions should be registered as events scheduled with proper internal between them to conform to the TPS requirement.

## Wait Queue
Workflow items blocked on state change of asynchronous operations must be handled efficiently. As an analogy, Linux manages threads that are waiting for some condition to become true using a wait queue. Similarly, workflows that are waiting for asynchronous operation to complete should be handled separately. The wait queue in the workflow engine should gather pending items, and perform state polling in a much slower rate than the workers. If notifications of completion can be obtained, wait queue should handle them as well. Once the asynchronous operation of a workflow item completes, the wait queue should remove it from the queue, and notify the worker.

Depend on the implementation of the worker, notification from the wait queue can either be event registered for worker handler in a event-driven design, or release of a mutex in thread-based worker. If data channels are available between threads, such as Go's channel between Go Routines, such notification can be done via the channel as well.

## Leak Fixer
Sometimes an operation cannot be made idempotent, and multiple invocations result in "leaked" resources. This is often the case when a resource is created or obtained to be used by the workflow. To avoid leaking, a Leak Fixer should be implemented. However, caution must be exercised as the data store that keeps records of resources used by the workflows can be eventual-consistent. If the count of existing resources is large, it is difficult to compare the list of existing resources with those in the data store. The best way to detect leaks is to change the resource from its initial state in the latter steps of the workflow, such as adding a tag or description, if possible.

# Testing
Because the following statements are true:
1. Each workflow state is well-defined;
2. Outcomes of each action is finite and well-defined;
3. Actions that can be taken on a workflow state is finite and deterministic.

Integration tests can be written for every outcome of every action between any state. Each test should consist three elements:
1. the state before the action is defined;
2. the behavior of rest of the world in this action is correctly mocked;
3. the state after the action is asserted, and operations of this action are asserted.

To ensure correctness of the workflow design, the coverage of such tests should be as thorough as possible. Because tests are rigorous in format, I recommend to spend some time to define the test infrastructure so that adding test cases becomes simple.
