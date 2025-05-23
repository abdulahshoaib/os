---
breadcrumbs: true
title: Semaphores
weight: 4
---
### Semaphore — Concept

A **semaphore** is a kernel-managed integer variable used to coordinate access to shared resources among concurrent processes or threads.
Two indivisible operations manipulate its value:

| Operation      | Conceptual Action                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------ |
| **wait (P)**   | Decrease the semaphore value. If it becomes negative, the caller is suspended.                   |
| **signal (V)** | Increase the semaphore value. If the new value is non-positive, one suspended caller is resumed. |

> Atomicity of each operation prevents race conditions on the semaphore itself.

## Implementation Styles

| Style                       | Core Idea                                                   | When a Process Must Wait                         | CPU Cost While Waiting                               | Typical Use Case                                                                                       |
| --------------------------- | ----------------------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Spinlock (busy-waiting)** | Caller repeatedly tests the semaphore in a tight loop.      | Spins inside user code or a short kernel loop.   | Consumes CPU cycles.                                 | Ultra-short critical sections on multiprocessors where a context-switch would cost more than the spin. |
| **Blocking (queue-based)**  | Caller is placed on a kernel queue linked to the semaphore. | Kernel deschedules the caller (`WAITING` state). | No CPU cycles consumed; context switch cost instead. | Longer critical sections or uniprocessor systems where CPU time must be preserved.                     |

> *Spinlocks favor latency; blocking semaphores favor overall CPU efficiency.*

## Binary vs Counting Semaphores

| Type                   | Initial Range            | Meaning of Value                                     | Main Purpose                                                                                       | Analogy                          |
| ---------------------- | ------------------------ | ---------------------------------------------------- | -------------------------------------------------------------------------------------------------- | -------------------------------- |
| **Binary Semaphore**   | `0` or `1`               | `1` ⇒ resource free, `0` ⇒ resource busy             | Acts like a **mutex lock** for a single shared resource or critical section.                       | Light switch: on or off.         |
| **Counting Semaphore** | Any non-negative integer | Value = number of identical resource units available | Controls access to a pool of interchangeable resources (buffer slots, database connections, etc.). | Parking lot counter: slots left. |

### Binary Semaphore Characteristics

* Ensures mutual exclusion for one resource.
* Only the first caller after a `signal` proceeds; others must wait.

### Counting Semaphore Characteristics

* Allows up to **N** concurrent accesses where **N** is the initial value.
* Becomes binary when initialized to **1**.

## Functional Roles

| Responsibility                        | How a Semaphore Fulfills It                                                                                                      |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Mutual Exclusion**                  | A binary semaphore guards entry and exit of a critical section.                                                                  |
| **Resource Counting**                 | A counting semaphore tracks how many identical resources remain; callers wait when none are free.                                |
| **Producer–Consumer Synchronization** | Two counting semaphores (`empty`, `full`) plus one binary semaphore (`mutex`) coordinate buffer space and mutual exclusion.      |
| **Ordering / Signaling**              | A semaphore initialized to `0` can force one thread to pause until another issues a `signal`, establishing sequence constraints. |

Semaphores thus provide a foundational, hardware-independent mechanism for coordinating concurrent activities in operating systems and multithreaded programs.

