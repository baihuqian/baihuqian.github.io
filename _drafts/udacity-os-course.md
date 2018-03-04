---
layout: "post"
title: "Udacity OS Course"
date: "2018-03-03 22:04"
---

{{:toc}}

# Introduction to Operating System

OS abstracts and arbitrates the computer hardware.
- have direct privileged access to the underlying hardware
- hide hardware complexity
- resource management
- provide isolation & protection between different processes

OS abstraction: process, thread, file, socket, memory page

OS mechanisms: create, schedule, open, write, allocate

OS policies: least-recently-used, earliest deadline first, etc.

#### OS design principles
- Separation of mechanism and policy (implement flexible mechanisms to support many policies)
- Optimize for common case

#### OS protection boundary
Privilege mode in kernel mode has direct access to hardware. User-kernel switch is supported by modern hardware. For example, there is a privilege bit in CPU and can be set by OS. If it is set in user mode, trap instructions are triggered and CPU is switched to privileged mode to allow OS to check whether this privileged access is allowed.

User level application can request OS to perform some privileged operation for them such as mmap (allocate memory). This is system call. It will involve mode change from user -> kernel -> user, and context switch requires saving state for user mode, swapping hardware cache, and executing a number of instructions.
In short, user/kernel switch is not cheap.

OS services are made available via system calls. Examples of OS services:

- Process control
- File manipulation
- Device manipulation
- Information maintenance
- Communication
- Protection

#### OS type
- Monolithic OS: one piece OS that contains everything
- Modular OS (Linux): more common. OS requires a certain interface that any modules should implement
- Microkernel: very basic kernel with only address space and threads. Everything else like file system, disk driver, runs out of kernel as user process. It requires a lot of inter-process interaction.

# Process and Process Management
Process: instance of an executing program, similar to "task", "job".
- State of execution: program counter, stack
- parts & temporary holding area: data
- May require special hardware: I/O

Application v.s. process:
- Application is static, stored in memory
- Process is dynamic, a representative of an running application

Address space, represented from $V_0$ to $V_{max}$ (virtual address), is used to represent the "in-memory" state of a process.
- Text and data: static state when process first loads. It starts from $V_0$.
- Heap: dynamically created during execution. It is after text and data.
- Stack: LIFO queue, grows from $V_{max}$ downwards.

Page tables map virtual address to physical address located in the physical memory.

#### Process Execution State
The OS maintains a Process Control Block (PCB). It is a data structure that OS maintains for every process.

- Process state
- Process number
- Program counter (PC): at which point the program is being executed. It is maintained in the CPU register.
- Registers
- Memory limits
- List of open files
- Priority
- Signal mask
- CPU scheduling info
- Stack pointer: represents the top of the stack
- others


PCB is created when process is created. Certain fields are updated when process state changes, but other fields change too frequently (like PC). CPU has a PC register that holds the PC for the current running process. It is the OS's job to update PC in its PCB when a process is no longer running on the CPU.

#### Context Switch
Switching the CPU from the context of one process to the context of another.

Context switch is expensive:
- Direct costs: number of cycles for load & store PCB
- Indirect costs: cold cache that leads to cache misses

####  Process Lifecycle
Processes can be running or idle (ready). Scheduler can dispatch it to CPU so it becomes running, and running processes can be interrupted. Process lifecycle is shows as follows:
![Alt text]({{ "/img/posts/udacity-os-cource/process_lifecycle.png" | absolute_url }})

Process can create other processes, forming a tree of processes. Process is created by fork or exec:
- Fork: copies the parent PCB into new child PCB. Child continues execution at instruction after fork (PC is copied into child PCB)
- Exec: create a new PCB and load a new program and start from first instruction.

On UNIX-based OSs, `init` is the "parent of all processes".
On the Android OS, `zygote` is the parent of all app processes.

#### Scheduler
A CPU scheduler determines *which one* of the currently ready processes will be *dispatched* to the CPU to start running, and *how long* it should run for.
- Preempt: interrupt and save the current context
- Schedule: run scheduler to choose the next process
- Dispatch: dispatch the chosen process nad switch into its context

**Efficiency**:

$ \eta_{CPU}=\frac{T_{processing}}{T}=\frac{T_{process}}{T_{process}+T_{scheduler}}$

Scheduler design questions:
- What are appropriate timeslice ($T_{process}$) values?
- Metrics to choose next process to run?

#### Inter Process Communication (IPC)
- Transfer data/info between address spaces
- Maintain protection and isolation
- Provide flexibility and performance

**Message-passing IPC**: OS provides and maintains communication channel, like a shared buffer. Processes write (send) or read (recv) message to/from the channel.
+: OS managed
-: overhead

**Shared memory IPC**: OS establishes a shared channel and maps it into each process address space. Processes directly read/write from this memory.
+: OS is out of the way
-: re-implement code to use the shared memory in the correct way, and set up for shared memory can be expensive

# Threads and Concurrency
[Reference](https://s3.amazonaws.com/content.udacity-data.com/courses/ud923/references/ud923-birrell-paper.pdf)

#### Process vs. Thread
- Process is defined by a PCB.
- Threads share the same code, data, and files, but they execute different pieces of the program, so each one of them has its own registers (such as PC) and stack.
- PCB for multi-threaded application is more complex. It has shared data, and per-thread exec context.

#### Benefits
- Parallelization: speed up the application
- Specialization: each thread dedicates to a small task, so its easy to manage, and cache efficiency can be improved
- Compared to multi-processes application, multi-threaded application is more memory efficient, and inter-thread communication is much more efficient than inter-process alternatives.
- Context switch for threads is quicker than that for processes.
- Multi-threaded OS can run execution contexts for multiple processes at the same time, and run OS-level services like daemons and drivers concurrently, on multi-processor hardware.

## Thread Mechanisms
#### Creation
Three key abstractions: thread type, fork, and join.

**Thread type**:
- ID
- PC
- SP (stack pointer)
- registers, stack, attributes

`fork(proc, args)` creates a new thread. PC points to the beginning of the procedure `proc`, and `args` are copied into its stack. It is **NOT** the unix `fork`, which is used to create new process.

`join(thread)` terminates a (child) thread.

#### Mutual Exclusion
**mutex** is a lock used for shared memory area. At any time, only one thread can acquire the mutex, and others must wait on acquiring mutex. The protected part of program is called *critical section*.

The specification of mutex doesn't guarantee the order of access granted for pending requests in its queue.

#### Condition Variable
Some threads wait on a certain condition to be true, while others send a signal when such condition is changed.

* Condition: a type
* `wait(mutex, condition)`: blocks a thread's execution until condition is met. The mutex is automatically released and re-acquired when entering and exiting wait
* `signal(condition)`: notify only one thread waiting on the condition
* `broadcast(condition)`: notify all waiting threads

Conditional Variable contains:
* reference to the mutex
* waiting threads
* (others)

#### Readers/Writer Problem
There is a shared resource. 0 or more Readers can access it at any time, but only 0 or 1 Writer can access it.

A mutex around the shared resource is OK, but it does not allow multiple readers to access at the same time.

Solution: resource counter
* free: resource_counter = 0
* reading: resource_counter > 0
* writing: resource_counter = -1

Resource variable is a proxy to the actual resource. Instead of controlling the resource, the resource variable is controlled.

#### Critical Section Structure

If a proxy is used (i.e. transform multiple states into a state variable guarded by a single mutex), the code block becomes:
```
// ENTER CRITICAL SECTION
perform_critical_operation
// EXIT CRITICAL SECTION
```

A piece of critical section code is executed when entering and exiting the *actual critical section*. But the actual critical section is not locked.
```
// ENTER CRITICAL SECTION
Lock(mutex) {
	while(!predicate_for_access)
		wait(mutex, cond_var)
	update_predicate
} // unlock

// Execute the actual critical section

// EXIT CRITICAL SECTION
Lock(mutex) {
	update predicate
	signal/broadcast(cond_var)
}
```

Critical section shares the following structure:
```
Lock(mutex) {
	while(!predicate_indicating_access_ok)
		wait(mutex, cond_var)
	update state => update predicate
	signal and/or broadcast(cond_var_with_correct_waiting_threads)
} // unlock
```

## Deadlock
Two or more competing threads are waiting on each other to complete, but none of them ever do.

A cycle in the wait graph is necessary and sufficient for a deadlock to occur. Edges in wait graph is from thread waiting on a resource to thread owning a resource.

## Kernel v.s. User-Level Threads
User-level threads must be associated with Kernel-level threads in order to execute. Kernel level scheduler schedules kernel-level threads onto the actual CPU.

### Relationship model between Kernel and User-Level Threads
#### One-to-One Model
User-level threads are directly mapped to kernel-level threads.

Pros:
* OS sees and understands user level threads;
* User-level threads can benefit from native support of thread functionalities such as synchronization and blocking.

Cons:
* All thread management must go to OS (may be expensive);
* OS may have limits on thread number and policies for thread management;
* Application may not be ported to OS with limited thread management.

#### Many-to-One Model
All threads of a process are mapped onto a single kernel-level threads. There is a user-level thread management library to manage threads in this process.

Pros:
* Total portable, independent to OS;
* Fewer user-kernel switch.

Cons:
* OS has no insights into application needs;
* OS may block the entire process if one user-level thread blocks on I/O.

#### Many-to-Many Model
Some threads are mapped to single kernel threads, and others are mapped to their own kernel level threads.

Pros:
* Get the best of both worlds;
* Can have bound (one-to-one) or unbound threads. Bound threads have better responsive time.

Cons:
* Requires coordination between user- and kernel-level thread managers.

### Multithreading Scope
* Process scope: user-level library manages threads within a single process;
* System scope: system-wide thread management by OS-level thread managers.

### Multithreading Pattern
#### Boss/Worker Pattern
One boss threads and a number of worker threads.
* Boss: assigns work to workers
* Worker: performs entire task

Throughput is limited by boss thread.

Boss assigns work by:
* Directly signalizing specific worker
	* Pros: workers don't need to synchronize
	* Cons: Boss must tract what each worker is doing; throughput will go down
* Preferred: Place work in producer/consumer queue
	* Pros: boss doesn't need to know details about workers; higher throughput
	* Cons: queue synchronization

How many workers?
* On demand (high overhead), not recommended
* Static pool of workers
* Dynamic pool of workers, adjusted by 1 at most

Pros: simplicity
Cons:
* Thread pool management (and task queue)
* Locality (workers will be more efficient to work on similar tasks): can be achieved with specialized workers

#### Pipeline Pattern
Tasks are divided into subtasks. Threads are assigned one subtask in the system, so entire tasks is not a pipeline of threads.

Multiple tasks run concurrently in the system, and in different pipeline stages.

Its throughput is the weakest subtask. So we can use a thread pool for each pipeline stage, so that each state can process the same number of tasks within a period of time.

Between stages, communication is done via shared buffer, such as queue.

Pros: specialization and locality
Cons: balancing and synchronization overheads

#### Layered Pattern
Each layer is a group of related subtasks, and end-to-end task must pass through all layers. It's less fine-grained than the pipeline model.

## Case Study: PThread
PThread = POSIX Thread (POSIX = Portable Operating System Interface)

### Compilation
Require
```cpp
#include <pthread.h>
```

Compile with `-lpthread` or `-pthread`:
```bash
gcc -o main main.c -lpthread
```

### PThread Creation
* Thread type: `pthread_t` is a data structure.
* Fork: `int pthread_create(pthread_t * thread, const pthread_attr_t * attr, void * (*start_routine)(void *), void (*.arg));` The `start_routine` is the execution (like `proc`), `arg` is the argument list. `pthread_attr_t` is to specify certain attributes of the thread. It returns the status code to indicate whether the creation is successful.
* Join: `int pthread_join(pthread_t thread, void ** status)`.  It returns the status code as well.

Pthread attributes:
* stack size
* scheduling policy
* priority
* inheritance
* joinable
* system/process scope

They have default behavior with NULL in `pthread_create`.

There are `int pthread_attr_init(pthread_attr_t * attr);`, `int pthread_attr_destory(pthread_attr_t *attr);` methods to create and destroy pthread attribute, and `pthread_attr-{set/get}(attribute)` as setters/getters.

Joinable: default to be joinable, meaning the parent thread cannot be terminated before its children threads joined, otherwise these children threads become zombies.
If set to false, the child thread becomes detached threads that can run on its own. PThread provides methods to detach a thread after creation, or create detached threads at creation time.

### PThread Mutexes
Mutex type: `pthread_mutex_t`

```cpp
// explicit lock
int pthread_mutex_lock(pthread_mutex_t *mutex);
// explicit unlock
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

Other mutex operations:
`int pthread_mutex_init(pthread_mutex_t * mutex, const pthread_mutexattr_t *attr)`. Its attributes is to specify mutex behavior when a mutex is shared among **processes** (by default a mutex is within a process).

`int pthread_mutex_destroy(pthread_mutex_t *mutex)`

`int pthread_mutex_trylock(pthread_mutex_t *mutex)`. It will not block the calling thread if the lock is not available.

### PThread Condition Variables
Conditional variable type: `pthread_cond_t`

`int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t * attr);`

`int pthread_cond_destroy(pthread_cond_t *cond);`

`int pthread_cond_watit(pthread_cond_t *cond, pthread_mutex_t *mutex)` When the thread wakes up upon condition, it will automatically reacquire the mutex.

`int pthread_cond_signal(pthread_cond_t *cond);`

`int pthread_cond_broadcast(pthread_cond_t *cond)`

# Thread Design Considerations
[Reference 1](https://s3.amazonaws.com/content.udacity-data.com/courses/ud923/references/ud923-eykholt-paper.pdf)
[Reference 2](https://s3.amazonaws.com/content.udacity-data.com/courses/ud923/references/ud923-stein-shah-paper.pdf)

## Thread-related Data Structures
User-level thread (ULT) contains data managed and used by thread library:
* UL thread ID
* UL register
* thread stack

Information of kernel-level thread is maintained in PCB. To support multiple kernel-level threads, PCB contains multiple stacks and registers for each kernel-level thread (KLT), but only one virtual address mapping per process, etc. Thus process state is broken into:
* "Hard" process state: virtual address mapping
* "Light" process state: association between ULT and KLT, such as signal mask, system call arguments, etc.

In a multi-core environment, data structure for each CPU is added to store pointers to current and other threads running or scheduled to run on the particular CPU.

Relationship between ULT, PCB, KLT, and CPU should be properly maintained.

Given the complexity of the data structure, it makes more sense to break PCB into multiple, small data structures.

### Case study: Solaris
#### User-level Thread Data Structures
![Alt text]({{ "/assets/posts/udacity-os-course/Solaris-user-level-thread-ds.png" | absolute_url }})
* not POSIX threads, but similar
* thread creation returns thread ID (tid), which is the index into a table of pointers to the actual thread data structure
* stack growth is unbounded -> dangerous! -> red zone -> system error when reaching the red zone

Thread local storage: static variables in thread functions, known at compile time

Size of the data structure is almost known at compile time, so it's better for locality

#### Kernel-level Thread Data Structures
It is divided into four parts: process (PCB), light-weight process (LWP), kernel-level-threads (KLT), and CPU. Their context and relationship is shown as below.

![Alt text]({{ "/assets/posts/udacity-os-course/Solaris-kernel-level-thread-ds.png" | absolute_url }})

![Alt text]({{ "/assets/posts/udacity-os-course/Solaris-kernel-level-thread-ds2.png" | absolute_url }})
This one doesn't show the corresponding CPU.


## Thread Management
If the ULT and KLT is not visible to each other, it creates overhead between the UL scheduler and KL scheduler. Special system calls must be made to allocate additional KLT, assign UL LWP to it, and release idle KLT after execution is done.

When there are multiple CPUs in the system, interaction is not between UL scheduler, KLT, and CPU. It becomes more complicated.

On multi-CPU system, it may take more time to context switch than spinning CPU cycles for mutex when mutex is held by a thread on another CPU. We call such mutexes "adaptive mutexes".

When destroying threads, sometimes it is more efficient to reuse them (to avoid allocating memory). When a thread exits,
* put them in a "death row"
* periodically destroyed by a reaper thread
* reuse thread structures and stacks if possible

## Interrupt and Signal
### Interrupt
* Events generated externally by components other than the CPU (I/O devices ,timers, other CPUs).
* It is delivered to a CPU.
* It is determined by the physical hardware.
* It appears asynchronously.
* It has a unique ID on the hardware.
* It triggers the corresponding handler set for the entire system by OS.
* It can be disabled by interrupt mask.

Interrupt is sent via physical connection to CPU, usually *in-band* (control signal is sent in the same channel as data signal, compared to *out-of-band*). It contains the interrupt ID, or MSI (Message Signaled Interrupts).

Interrupt handler table stores handlers of supported interrupts. While the interrupts are system-wide, the handler is defined in the OS.

Interrupts can be directed to any CPU with it enabled. Thus to avoid overheads and perturbations, it makes sense to set interrupt on just a single core.

### Signals
* Events triggered by the CPU and software running on it.
* It is determined by the running OS.
* It can appear both synchronously and asynchronously.
* It has a unique ID on OS.
* It triggers the corresponding handler set by process.
* It can be disabled by signal mask.

A signal can be generated when a process/thread accesses kernel-level resources, and other events.

OS provides default handling of signals, if a process doesn't provide it. Default actions include terminate, ignore, terminate and core dump, stop, or continue.

Process can install handlers via `signal()` or `sigaction()` method call. However, some signals cannot be caught by the process.

Example:
Synchronous:
* SIGSEGV (access to protected memory)
* SIGFPE (divided by zero)
* SIGKILL (kill by id): can be directed to a specific thread

Asynchronous:
* SIGKILL (kill)
* SIGALARM

* One-shot signals: n signals pending is equal to 1 pending signal. The handler must be re-enabled after invocation.
* Real time signals: handler is invoked every time the signal is raised.

### Handler
Handler operates on the current thread. It will cause implementation difficulties if the handler code is complex. If the handler requires synchronization (acquire locks), then it cannot run on the current thread if the lock is held by the current thread. Instead, it has to run a different thread.

### Mask
sequence of 0/1 bits, 0 means disabled and 1 is enabled. If a interrupt/signal is disabled, it will be stored and handled later when it is enabled.

Interrupt masks are per CPU. If mask disables interrupt, the hardware interrupt routing mechanism will not deliver interrupt to CPU.

Signal masks are per execution context (ULT on top of KLT). If mask disables signal, kernel will not deliver it to the corresponding thread.

## Thread Performance Considerations
### Performance Metrics
Metric matters! When evaludating performance, think carefully about what metric to use.

Metrics: a measurement standard
* measurable and/or quantifiable property
* of the system we are interested in
* can be used to study system behavior

Example:
* wait time
* throughput
* request rate
* execution time
* utilization
* platform efficiency (resources used to achieve certain throughput)
* performance / $, performance / w, etc.
* percentage of SLA violations

### Multi-process v.s. Multi-threading
**Multi-process:**
pro: simple programming
cons:
* high memory usage
* costly context switch
* hard/costly to maintain shared state

**Multi-threading:**
pros:
* shared address space
* shared state
* cheap context switch

cons:
* not simple implementation
* requires synchronization
* underlying OS should support for threads

Multi-process and multi-thread serves one request per context.

### Event-driven model
A dispatch loop continuously checks incoming events, and dispatches them to event handlers. Dispatcher is like a state machine, and call handler is like jump to a specific place in code (not context switch). Handler surrenders control over blocking operations so that other event handlers can be executed.

Many requests interleaved in an execution context. It processes request until waiting is necessary (blocking IO, etc.), then it switches to another request. Thus the number of context switch is minimized.

Multiple CPUs can utilize multiple event-driven processes.

**Q: How request dispatcher finds incoming event?**
Event is input on file descriptor (FD). There is a fixed set of FD to scan through.

**APIs to scan FD:**
* `select`: allow a program to monitor multiple file descriptors, waiting until one or more of the file descriptors become "ready" for some class of I/O operation (e.g., input possible).
* `poll`: performs a similar task to `select`. It waits for one of a set of file descriptors to become ready to perform I/O.
* `epoll`: performs better than `poll`.

**Benefit of event-driven model:**
* single address space
* simple flow of control
* smaller memory requirement
* no context switching
* no synchronization

### Asynchronous I/O calls
**Problem: Blocking I/O calls will block the entire event-driven loop.**
Use non-blocking, i.e. asynchronous I/O.

1. Process/thread makes a system call
2. OS obtains all necessary information from stack, and either learns where to return results, or tells caller where to get results later
3. Process/thread can continue.

It requires kernel-level thread support (i.e. multi-threaded kernel), and/or device support (e.g. DMA).

#### Helpers
**Q: What if Async calls are not available?**
Helpers, threads/processes designed for blocking I/O only. It uses pipe/socket based communication channel with the event dispatcher (`select` or `poll` still works), so that the main event loop does not block.

It resolves portability of basic event-driven model where Async I/O is not available, but it has a smaller footprint than regular worker thread design.

# Scheduling
[Reference](https://s3.amazonaws.com/content.udacity-data.com/courses/ud923/references/ud923-fedorova-paper.pdf)

CPU scheduler decides when and how processes (and their threads) access shared CPUs.
* chooses one of the ready tasks (from ready queue) to run on CPU
* runs when 1) CPU becomes idle, 2) new task becomes ready, 3) timeslice expired

Two key aspects of scheduler:
* Scheduling algorithm: which task should be executed
* Runqueue: how it is done

Metrics:
* throughput
* average job completion time
* average job wait time
* CPU utilization

## Scheduling Algorithm
### Run-to-Completion Scheduling
Assumption:
* tasks/jobs that require scheduling are grouped
* known execution times
* no preemption
* single CPU

#### First-come-first-serve
Algorithm: FIFO
Runqueue: normal queue

#### Shortest Job First
Algorithm: shcedules tasks in order of their execution time
Runqueue: priority queue (heap)

### Preemptive Scheduling
Estimation of job execution time:
* how long did a task run last time?
* how long did a task run last n times (windowed average)?

#### Shortest Job First
Preempt longer running jobs (in terms of remaining time) for shorter jobs (just arrived)

#### Priority Scheduling
Preempt running jobs with lower priority for high priority jobs.

Runqueue: one priority queue for each priority level.

Danger: starvation of low priority tasks.
Solution: priority = f(actual priority, time spent in runqueue). The longer a task is in the runqueue, the higher its priority is.

Priority Inversion: if a higher priority task requires mutex held by low-priority tasks, it is executed **after** the low priority task finishes and releases the mutex.
Solution: temporarily boost priority of mutex owner so that it can release mutex sonner.

#### Round-Robin Scheduling
Pick up the first task from queue. If a task yields because blocking operations like I/O, it is placed at the end of run queue, and the next task in the runqueue is scheduled.

#### Timeslicing
Each task can run for a maximum amount of time. When the timeslice is expired, the task is returned to the end of runqueue. Tasks may run less than timeslice time (I/O, yield due to priority)

Pros:
* shorter tasks finish sooner
* more responsive
* lengthy I/O operations are initiated sooner

Cons:
* overhead for context switch (thus timeslice >> context switch time)

For CPU bound tasks, larger timeslice is better. (context switch overheads are limited, allowing higher CPU utilization on real tasks)
For IO bound tasks, smaller timeslice is better. (I/O operations are issued earlier, both CPU and device utilizations are higher)

## Runqueue Structure
Use separate queue structure for I/O, mixed, and CPU-bound tasks with different timeslices and priorities.

How to know what type of tasks?
Solution: Multi-Level Feedback Queue (MLFQ)
![Alt text]({{ "/assets/posts/udacity-os-course/multi-level-feedback-queue.png" | absolute_url }})

1. tasks enter topmost queue
2. if a task yields voluntarily before timeslice expires, keep the task at current level; if task uses entire timeslice, push it down to lower level;
3. task in lower level gets priority boost when releasing CPU due to I/O waits

## Example
### Linux O(1) Scheduler
Constant time to select/add task, regardless of task count

Preemptive, priority based: 140 priority level (0-139), 0-99 are real-time, and 100-139 are timesharing. User processes' default priority is 120, but their "nice value" can be set to -20 to 19 to adjust priority.

Timeslice value depends on priority. Smallest timeslice for lowest priority.
Feedback based on waiting/idling: longer sleep time means more interaction with devices, priority - 5 (boost); shorter sleep means compute-intensive, priority + 5 (lowered).

Runqueue is 2 arrays of tasks. Each array element represents a priority level, and points to the first of a list of tasks at this level.

Active array:
* used to pick the next task to run
* constant time to add/select (constant time to detect first non-empty array slot, and indexing with priority)
* tasks remain in queue in active array until timeslice expires

Expired array;
* inactive list
* when no more tasks in active array, swap active and expired array

Problem:
* performance of interactive tasks is poor
* fairness

### Linux Completely Fair Scheduler
Runqueue: a **single** red-black tree (balanced search tree), ordered by "vruntime" (virtual runtime, in nanosec)

Algorithm:
* always pick leftmost node
* periodically adjust vruntime in the runqueue, and compare the current running task to leftmost node. If smaller, continue running; if larger, preempt and place the task back in the tree
* vruntime progress rate depends on priority and niceness: vruntime grow rate is lafster for low-priority

Performance
* select task: O(1)
* add task: O(logN)

## Scheduling on Multi-CPU Systems
OS sees CPUs and their cores as entities that it can schedule execution contexts on. Whether it is shared-memory multi-processor (SMP) or multi-core CPU, such system has a shared memory (DRAM), separate last-level cache (LLC) for each CPU/Core, and private on-chip caches for each CPU/Core.

Cache-affinity is important. Schedule a thread to a CPU where its cache is still hot, or keep tasks on the same CPU as much as possible.

Maintain per-CPU runqueue and scheduler, and tasks are load-balanced across CPUs based on per-CPU queue length or when CPU is idle.

If a system has multiple memory nodes, or "Non-Uniform Memory Access (NUMA)", access to local memory node is faster than that to remote memory node. Tasks should be kept on CPU closer to memory note where there state is, and this is called NUMA-aware scheduling.

### Hyperthreading
[Reference](https://s3.amazonaws.com/content.udacity-data.com/courses/ud923/references/ud923-fedorova-paper.pdf)

CPU needs context switching because it has only one set of registers to keep the execution context of a running thread. Hyperthreading provides multiple hardware-supported execution contexts (multiple registers) in one CPU, so that context switching is very fast.

Context switching in Hyperthreading is O(cycles) and memory access is O(100 cycles), hyperthreading can hide memory access latency.

Co-schedule compute- and memory-bound threads on the same core to avoid/limit contention on processor pipeline (compute) and balance out load on memory controller.

#### CPU-bound or Memory-bound?
Software determination of the characteristics of a thread is not good because of context switching cost. Hardware provides information about a thread via hardware counters:

* Cache misses, L1, L2, LLC misses
* IPC
* Power and energy data

Scheduler can read the hardware counters to estimate what kind of resources a thread needs. Models of well-understood workloads and per-architecture workloads are also used.

# Memory Management
OS decouples physical memory from virtual memory space used by processes. Memory management system must be able to allocate memory, replace content in the physical memory with secondary storage, and perform address arbitration such as translation and validation.

Virtual memory is divided into fixed size pages, and physical memory is divided into page frames of the same size. Allocation is done via a mapping from pages to page frames, and arbitration is done via page tables. This is called page-based memory management.

An alternative to page-based management is segment-based memory management. Instead of fixed-size pages, memory is allocated to variable-sized segments.

Hardware support for memory management is provided by Memory Management Unit (MMU). It sits between CPU and main memory that translates virtual to physical addresses, and report faults for illegal access, permission, or content not presented in memory. Special registers are also used to hold pointers to page tables to improve performance. Because VA-to-PA translation happens at every memory access, a cache can be used for valid translations. It is called Translation Lookaside Buffer (TLB). The actual translation is done by hardware.

## Page Tables
Page table is a map that tells OS and hardware where to find the virtual memory in physical memory. It's a mapping between Virtual Page Number (VPN) to Page Frame Number (PFN). There are offsets for each entry to decide the actual location in the page.

To reduce number of entries in page table, consecutive memory space can be combined into one entry. Memory is allocated on first touch, and can be reclaimed if not used. Reclaimed pages have valid bit set to 0. Reaccess will reestablish pages, but it will be in a different PFN.

Note page tables are per-process. Context switch will result in switching to another valid page table. It is done by updating a register (CR3 register in x86) with address to the page table.

#### Page Table Entries
Each entry has multiple flags besides PFN.
* **P**resent (valid/invalid)
* **D**irty (written to)
* **A**ccessed (read or write)
* protection bits: **RWX**

OS uses flags to determine the validity of access. Faults will be placed onto kernel stack and trap the CPU into kernel mode to handle the fault.

### Page Table Size
In 32-bit architecture, Page Table Entry (PTE) is 4 bytes, including PFN and flags. Virtual Page Number (VPN) is $2^32$ / page size. Common page size is 4 KB. So the size of the page table is 4 MB (per process).

In 64-bit architecture, Page Table Entry (PTE) is 8 bytes. If the page size is still 4KB, the page table will be 32 PB.

However, processes don't use the entire address space, but page table assumes an entry per VPN, regardless of whether the virtual memory is needed or not.

To save space, hierarchical page tables can be used. In a two-level design, outer page table or top page table is page table directory, and internal page table exists only for valid virtual memory regions. On malloc, a new internal page table may be allocated. This is important to 64 bit architectures as memory space contains larger and more sparse gaps.

### Translation Lookaside Buffer
Because memory access will require $n$ page table accesses ($n$ is the number of page table levels) and 1 memory access, multi-layer page table can introduce overhead. Modern MMU provides hardware cache for address translation, called Translation Lookaside Buffer (TLB), to improve memory access performance. TLB has all protection/validity bits to validate access.

Small number of cached address can result in high TLB hit rate, due to temporal and spatial locality. For example, x86 Core i7 processor has 64-entry data TLB and 128-entry instruction TLB per core, and 512-entry shared second-level TLB.

### Inverted Page Table
Because physical memory space is much smaller compared to virtual memory space, page table can be constructed with only allocated physical memory space. OS maintains a page table with one entry for each physical frame. Each entry contains the Process ID (PID), the first part of the virtual memory, and offset. To look up a memory address, a linear search is performed to find the entry, whose index is the physical frame number.

To speed up lookup, hashing can be used to construct a hashing page table. The logical address is hashed, and the hash value is the entry in the page table.

### Segmentation
Segments can be used to represent a contiguous physical memory with arbitrary granularity. It often corresponds with code, heap, data, stack, etc. Logical address is then segment selector and offset.

Segments must be supported by the hardware.

### Page Size
Different OS/hardware platform supports different pages sizes. For example, Linux/x86 platform supports 4KB, 2MB (large), and 1GB (huge) page sizes. Large pages can significantly reduce the page table size. But if the page is not fully used, internal fragmentation will occur, which wastes memory.

## Memory Allocation
Memory allocator determines VA to PA mapping. It exists at kernel-level as well as user-level. Kernel-level allocator allocates memory for kernel state and static process state. It also tracks the amount of free memory available to the system. User-level allocator manages dynamic process state (heap), and malloc/free operations.

Efficient memory allocation algorithm limits the possibility of external fragmentation (in which free memory space are fragmented and cannot be used by largest allocation request), and permits coalescing/aggregation of free areas.

### Linux Allocator
#### Buddy Allocator
It starts with $2^x$ area. On allocation request, it subdivides the memory space into $2^x$ chunks and find the smallest $2^x$ chunk that can satisfy the request. Fragmentation still exists, but on free it is easy to check to see if neighbors can be aggregated into a larger chunk, so aggregation works well and fast.

#### Slab Allocator
Internal fragmentation occurs when allocation requests are not close to $2^x$. Slab allocator builds caches for certain-size objects on top of contiguous memory, so that internal fragmentation can be avoided, and external fragmentation is no longer an issue. Each allocation request will get a space on the cache, or new cache will be created.

### Demand Paging
Because virtual memory is much larger than the physical memory, virtual memory page is not always in physical memory, and physical page frame can be saved and restored to/from secondary storage.

Demand paging is the process of having pages swapped in/out of memory and a swap partition (e.g. on disk).

1. On a memory access, page table's present bit indicates the page is not present in memory.
2. It traps into OS kernel.
3. OS determines the location of the page, and issue an IO request to retrieve the page.
4. The missing page is brought back to memory.
5. Page table is reset with the new physical memory location.
6. Memory access is returned.

if a page is "pinned", swapping will be disabled.

### Page Replacement
When should page be swapped out by page(out) daemon?
- when *memory usage* is above threshold
- when *CPU usage* is below threshold

Which pages should be swapped out?
Pick Least-recently used pages. It uses the Access bit to track if page is referenced. Other choices are pages that don't need to be written out, which can be tracked using the dirty bit.
