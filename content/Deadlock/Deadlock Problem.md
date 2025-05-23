---
title: Deadlock Problem
prev: Deadlock
weight: 1
---
## Concept

In a computer system, **deadlock** refers to a situation where a set of processes become stuck, each waiting for a resource that is currently held by another process in the same set. As a result, none of them can proceed, and unless something external intervenes (like manual killing of a process), the system remains in this stalled state indefinitely.

## Example with Semaphores

Consider two semaphores `A` and `B`, both initialized to `1` (i.e., available). Two processes, `P1` and `P2`, try to acquire them:

**Process P1:**

```c
wait(A);
wait(B);
```

**Process P2:**

```c
wait(B);
wait(A);
```

Here’s what could happen:

1. P1 gets semaphore `A` and is now holding it.
2. Simultaneously, P2 gets semaphore `B` and is now holding it.
3. P1 now wants `B`, but it’s held by P2 → **P1 is blocked**.
4. P2 now wants `A`, but it’s held by P1 → **P2 is blocked**.

Both are now blocked forever. This is a deadlock.

## The System Model for Deadlocks

A computer system consists of a set of resources that are shared among processes. These resources can include:

* **CPU cycles**
* **Memory**
* **Disk drives**
* **I/O devices** (like printers or network cards)

Each **resource type** may have one or more **instances**. For example, there may be multiple disk drives or only one printer.

Processes use resources through the following sequence:

1. **Request**: The process asks for the resource.
2. **Use**: The process does work with the resource.
3. **Release**: The process releases the resource.

If a resource is not available when requested, the process goes into a **waiting state** until the resource becomes available.

## Conditions for Deadlock

A deadlock can occur only if all the following four conditions are true at the same time in the system. These are known as the Coffman conditions:

### 1. Mutual Exclusion

Some resources cannot be shared. If one process is using a resource (like a printer), others must wait.

This is essential for correctness (e.g., two processes can’t print on the same printer at the same time), but it also introduces exclusivity — a precondition for deadlock.

### 2. Hold and Wait

A process is **holding one or more resources** and is waiting to acquire additional resources that are currently held by other processes.

For example, a process may have a lock on a file and then request access to the printer. If the printer is unavailable, the process will wait — while still holding the file lock.

### 3. No Preemption

Resources cannot be forcibly taken from a process. A resource can only be released **voluntarily** by the process that holds it after it has completed its task.

This means that if a process is holding a resource, no other process or even the OS can take it away — it must wait.

### 4. Circular Wait

There exists a **cycle** of waiting processes: each process in the cycle is waiting for a resource that the next process in the cycle holds.

Example:

* P1 waits for a resource held by P2
* P2 waits for a resource held by P3
* ...
* Pn waits for a resource held by P1

This closed loop creates a situation where no process can proceed, because each is waiting for another.
