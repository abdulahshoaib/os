---
title: Scheduling
next: "Scheduling Algorithms"
prev: "CPU Scheduling"
weight: 1
---
## CPU Scheduler

When the CPU becomes idle, the operating system must select one of the processes in the ready queue to execute next. This selection is done by the short-term scheduler, also called the CPU scheduler. It picks a process from memory that is ready to run and calls the dispatcher to allocate the CPU to that process. The ready queue may be implemented using different data structures like FIFO queues, trees, or unordered linked lists. Each entry in the ready queue is usually a process control block (PCB) representing a process.

## Dispatcher

The dispatcher is a part of the kernel responsible for switching the CPU from the currently running process to the one chosen by the CPU scheduler. This involves three key tasks:

1. Saving the context of the current process and restoring the context of the new process
2. Switching from kernel mode to user mode
3. Jumping to the appropriate instruction in the new process to resume execution

The total time required to perform this switch is known as dispatch latency.

```mermaid
sequenceDiagram
    participant CPU
    participant Process A
    participant Scheduler
    participant Process B

    Process A->>CPU: Currently running
    CPU->>Scheduler: Becomes idle or interrupted
    Scheduler->>Process B: Selects next process
    Scheduler->>CPU: Calls dispatcher
    CPU->>Process A: Saves context
    CPU->>Process B: Restores context and runs
````

## Preemptive and Non-Preemptive Scheduling

Scheduling can be either preemptive or non-preemptive depending on when the CPU is taken away from a process.

If a process moves from running to waiting (for example, during an I/O request) or terminates, the scheduler makes a decision without forcing an interruption. This is non-preemptive scheduling.

If a process moves from running to ready (due to an interrupt) or from waiting to ready (for example, I/O completion), then the currently running process may be interrupted to allow another to run. This is preemptive scheduling.

## Scheduling Criteria

CPU utilization measures how effectively the CPU is being used. The goal is to keep it busy as much as possible. In real systems, CPU utilization ranges from 40% on lightly loaded systems to 90% in heavily used environments.

Throughput refers to the number of processes completed per unit of time. A higher throughput indicates more work is being done, and is desirable.

Turnaround time is the total time from when a process is submitted to when it is completed. It includes time spent in memory allocation, waiting in the ready queue, executing on the CPU, and performing I/O. Minimizing turnaround time improves system responsiveness.

Waiting time is the amount of time a process spends waiting in the ready queue before execution. Reducing waiting time increases CPU efficiency and ensures quicker processing of tasks.

Response time is the interval between the submission of a request and the system’s first response. It does not include the time taken to complete the request, only the time to begin processing. Lower response time makes the system appear faster to users
