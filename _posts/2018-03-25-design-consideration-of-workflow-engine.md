---
layout: post
title: Design Consideration of a Workflow Engine
date: '2018-03-25 15:05'
tags:
 - System Design
published: true
---

In a distributed, service-oriented software system, it is often required to achieve some goals with a combination of one or more operations, such as API calls, database operations, or others. The series of operations are often called "workflows" and the software designed to drive the workflows to completion is called "workflow engine". Operations in workflows are remote to the workflow engine. Thus, it is possible to encounter failure, network glitch or latency, or the host running the software can fail. Thus, a good workflow engine design is vital to the correctness and the performance of the overall software system that it serves.

In this post, I attempt to lay out the design considerations of a good workflow engine.

* toc
{:toc}

## Workflow Modeling

So, what is a workflow?

There are formal definitions for a workflow. However, in my understanding, a workflow is a finite, ordered series of actions that, if performed in order, transforms the start state to the end state. The state of a workflow is the collection of states of resources, values of database entries, or states in remote systems. An action in a workflow is a mutation API call or a database operation. that changes at least one aspect of the state of the workflow. Non-mutating APIs, read-only database accesses are not treated as actions.

The workflow can be modeled as a directed acyclic graph with one or multiple paths between two special states: the initial state and the end state. Each possible state in the workflow is a node, and each action is an edge in the graph. The initial and end state of a workflow is well defined, and the consequence ("changeset") of each action is well-defined. Therefore, each workflow state is well-defined.

In practice, multiple independent actions (actions executed in arbitrary order) can be grouped into a single state to reduce the number of states in a workflow, increasing parallelism but the complexity of each state. This means the action in such workflows must handle partially-executed scenarios, and branch out to the appropriate execution paths. Due to the complexity of such solutions, it is out of the scope for this post.

# Design Challenges

Given the distributed nature of the software system, the workflow engine faces a series of challenges:

1.    The state of a workflow is distributed and is expensive to construct from the source of truth.
2.    Actions in a workflow are not always reliable. Because they go through networks, the request can be lost, the remote system can be unresponsive, the response can be lost, or the latency is too long that the client gives up before server returns the response. While it is quite unlikely, though possible, that a successful action does not result in a correct transition of states, a failed action often produces various changes to the state, including NO-OP (action is not taken), expected operation (action is taken, but the response is lost), and sometimes unexpected operation.
3.    The workflow engine can fail, so in-flight workflows must be picked up cleanly by another host running the same version or a different version of the application.
4.    If more than one host runs the workflow engine, workflows must be efficiently and correctly distributed to them to ensure one and only one host works on any workflows at any time.
5.    Actions are rate-limited by type. Proper rate limiting must be done to protect the workflow engine's dependencies.
6.    Some actions are asynchronous, and state transitions of such actions take time. The workflow engine must timely and efficiently discover state transitions.

# Design Principles    

To address the challenges as mentioned above, the following design principles should be considered:

## Action
1.    Each action should include only one mutating operation that, under normal circumstances, can either succeed as a whole or fail as a whole. This can be one API call or one database operation. Multiple operations can be grouped into one action if transactional locks can be used to rollback partially succeeded operations when following operations in the same action fail.
2.    After an action succeeds, the changeset of the workflow state is well-defined. There should be no ambiguity of the change action can apply.
3.    The failure mode of action is finite and well-understood.
4.    Any API calls and database operations should be rate-limited.
5.    Results of asynchronous actions should be retrieved timely and efficiently.
6.    Actions should be idempotent whenever possible. If an action cannot be idempotent, the workflow must ensure that either the action can be taken multiple times without side effect, the consequence of the extra calls does not impact the workflow state, or the consequence of extra calls can be efficiently cleaned up.

## Workflow State
1.    Workflow state should be cached in memory, and the next action can be taken without reconstructing the workflow state from the source of truth.
2.    Workflow state should be persisted in data store after each action completes so that when the workflow is assigned to a different host, its state can be efficiently constructed without consulting the source of truth.
3.    If different actions can be taken based on external conditions, these conditions are well-defined and their true state can be efficiently discovered.

# Design
A workflow engine can be divided into the following functional units.

## Workflow Worker
Workflow workers perform actions and state transitions.

There are three paradigms of designing workflow workers.
1. "Dedicated worker" paradigm: Each worker works on only one workflow item until this item completes.
2. "Assembly line" paradigm. Each worker works on a small set of actions, and a workflow item is sequentially worked by multiple workers in a defined order.
3. "Dumb worker" paradigm: Workers are agnostic to the workflow item or the specific action; they receive the workflow state and the action, execute the action and record the state change, and move on to the next one, which can be the same workflow item, a different workflow item in a different state, or even a different type of workflow.

There are both advantages and disadvantages in all paradigms. In the dedicated worker paradigm, workflow state is maintained by a single worker, eliminating the need of passing state from one worker to the next. All the actions of this workflow must be defined in a single place, which can be good or bad depending on the situation. However, this process does not allow batch processing, and thus a workflow scheduler that distributes workflows to workers is required.

In the assembly line paradigm, workflow actions are spread to multiple workers, which can reduce the readability of the code. It also requires passing workflow state between workers. It makes adding or removing workflow states complicated. However, batch processing is possible.

In the dumb worker paradigm, workers are dead simple. However, workflow management must be done carefully. The state of all in-progress workflows must be tracked so that no workflows are left unworked, or state gets stale. The workflows must be distributed to workers according to carefully-calibrated policies, or the time-to-completion of workflows can vary greatly based on the load in the system.

I recommend leveraging dedicated worker paradigm because:
1. The workflow is defined in one place.
2. The number of in-flight workers is the number of in-flight tasks, and the age of workers is the age of the task (plus some initialization delay).
3. The modification of a workflow is confined in one place.

## Workflow Scheduler
To ensure workflows are organized in a desirable way, whether to ensure fairness between workflow items, to prioritize in-flight items over new items, or to distribute workflow items to multiple hosts running the workflow engine to avoid contention, a workflow scheduler is helpful to manage workers.

In the assembly line paradigm, back pressure must be implemented by the scheduler so that the speed of each worker is limited to the slowest one, to ensure no accumulation of workflow items at the bottleneck. In the dedicated worker paradigm, the number of concurrent workers should be limited so that no dependency is overwhelmed.

In the dumb worker paradigm, a scheduler is a must. I will not dive into how the scheduler should be implemented, because we can get inspiration from the OS scheduler. If you think a workflow item as a thread, its state is the thread stack, and its next action is the combination of the program counter and the machine instruction, then you can almost derive a design from various OS schedulers. However, given the complexity of OS scheduler and the often-distributed nature of the workflow engine, I don't recommend this model.

## Rate Limiter
There is always a limit of capacity for any dependencies. This is often defined as allowed Transaction per Second, or TPS, from a given service. Thus, to efficiently use the CPU and network bandwidth, the workflow engine must implement rate limiter at the client side.

The implementation of Rate Limiter depends on the design of the worker. If workers are designed as threads, then the threads must be paused when the allowed TPS is exceeded. If workers are designed as event handlers, API calls or database transactions should be registered as events scheduled with a proper interval between them to conform to the TPS requirement.

Note, there is no perfect rate limiter, and in extraordinary circumstances, services may not honor pre-determined TPS. Even rate limiter exists, the workflow engine must be able to handle throttled requests with proper back-off and retry mechanism.

## Wait Queue
Workflow items blocked on state changes of asynchronous operations must be handled efficiently. As an analogy, Linux manages threads that are waiting for some condition to become true using a wait queue. Similarly, workflows that are waiting for the completion of asynchronous operations should be handled separately. The wait queue in the workflow engine should gather all pending items and perform state polling at a much slower rate than the workers. If notifications of completion can be obtained, wait queue should handle them as well. Once the asynchronous operation of a workflow item completes, the wait queue should remove it from the queue, and notify the worker.

Depend on the implementation of the worker, notification from the wait queue can either be event registered for worker handler in an event-driven design, or release of a mutex in a thread-based worker. If data channels are available between threads, such as Go's channel between Go Routines, such notification can be done via the channel as well.

It is worth calling out that the assembly line paradigm has two advantages for waiting:
1.    Because workers in the assembly line paradigm work on batches, wait tasks are naturally aggregated without additional wait queue mechanisms. This can either reduce system complexity or polling workload.
2.    Workers in the assembly line paradigm work on other tasks while waiting on a task, saving CPU cycles.

## Leak Fixer
Sometimes an operation cannot be made idempotent, and multiple invocations result in "leaked" resources. This is often the case when a resource is created or obtained to be used by the workflow. To avoid leaking, a Leak Fixer should be implemented. However, caution must be exercised as the data store that keeps records of resources used by the workflows can be eventual-consistent. If the count of existing resources is large, it is difficult to compare the list of existing resources with those in the data store. The best way to detect leaks is to change the resource from its initial state in the latter steps of the workflow, such as adding a tag or description, if possible.

## Instrumentation, Logging, and Monitoring
With states of workflows held in memory, it is hard to troubleshoot when issues coming up. Thus, it is necessary to add sufficient instrumentation and logging to the workflow engine. Additionally, metrics and real-time log processing can improve the monitoring and visibility to the service.

# Testing - Workflows
Because the following statements are true:
1. Each workflow state is well-defined;
2. Outcomes of each action are finite and well-defined;
3. Actions that can be taken on a workflow state is finite and deterministic.

Integration tests can be written for every outcome of every action between any state. Each test should consist of three elements:
1. the state before the action is defined;
2. the behavior of rest of the world in this action is correctly mocked;
3. the state after the action is asserted, and operations of this action are asserted.

To ensure the correctness of the workflow design, the coverage of such tests should be as thorough as possible. Because tests are rigorous in format, I recommend spending some time to define the test infrastructure so that adding test cases becomes simple.

In practice, the following categories of tests should be written:
1.    Sequential workflow tests. Because the state after the previous action is the state before the current action, tests can be chained together to simulate a complete run of a particular workflow under “happy” cases.
2.    Failure mode tests. Because various failure modes cannot be thoroughly covered in implementation and indeed be discovered at runtime, test cases should be written to cover most of them. The test should utilize the test infrastructure defined above with 1) the pre-state of the failed action, 2) the behavior of remote systems that causes the action to fail, and 3) the expected outcome of the action when correctly handling the failure.
3.    Idempotency tests. Special tests can be written to deliberately run each action twice and validate the side effect is the same as one action, or exceptions are correctly handled.
4.    Race condition tests. Two different workflows should be run concurrently and all possible interleaving should be tested.

# Testing - Workflow Engine
Besides integration tests of the actual workflow, the workflow engine code must be thoroughly tested as well. It is recommended that each component in the workflow engine is tested separately while its interaction with other components are mocked.
