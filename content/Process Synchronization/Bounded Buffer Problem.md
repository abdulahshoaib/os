---
breadcrumbs: true
title: Bounded Buffer Problem
weight: 5
---
## Problem Statement

Two concurrent processes share a fixed-size circular buffer:

| Role         | Action                                           |
| ------------ | ------------------------------------------------ |
| **Producer** | Generates items and places them into the buffer. |
| **Consumer** | Removes items from the buffer for use.           |

Because the buffer is **bounded**, the producer must wait when it is **full**, and the consumer must wait when it is **empty**. Correctness requires mutual exclusion while either process accesses the shared indices or buffer slots.

## Solution

### Synchronization Primitives

| Semaphore | Initial Value | Meaning                                                           |
| --------- | ------------- | ----------------------------------------------------------------- |
| `mutex`   | `1`           | Binary semaphore ensuring **mutual exclusion** for buffer access. |
| `empty`   | `N`           | Counting semaphore tracking **free slots** (`N` = buffer size).   |
| `full`    | `0`           | Counting semaphore tracking **filled slots**.                     |

All semaphore operations (`wait`, `signal`) are atomic.

### Shared Data

```c
#define N 10
item buffer[N];
int  in  = 0;      // next free position
int  out = 0;      // next filled position

semaphore mutex = 1;   // mutual exclusion
semaphore empty = N;   // initially N slots are empty
semaphore full  = 0;   // initially 0 items are full
```

### Producer

```c
while (true) {
    item x = produce_item();      // generate item

    wait(empty);                  // decrement empty count (block if 0)
    wait(mutex);                  // enter critical section

        buffer[in] = x;           // insert item
        in = (in + 1) % N;        // advance index circularly

    signal(mutex);                // leave critical section
    signal(full);                 // increment full count
}
```

### Consumer

```c
while (true) {
    wait(full);                   // decrement full count (block if 0)
    wait(mutex);                  // enter critical section

        item y = buffer[out];     // remove item
        out = (out + 1) % N;      // advance index circularly

    signal(mutex);                // leave critical section
    signal(empty);                // increment empty count

    consume_item(y);              // use item
}
```

## How the Semaphores Coordinate the Processes

| Step                      | Semaphore Action                | Effect                                                          |
| ------------------------- | ------------------------------- | --------------------------------------------------------------- |
| **Space check**           | `wait(empty)` (Producer)        | Blocks if buffer is full; otherwise reserves one slot.          |
| **Data check**            | `wait(full)` (Consumer)         | Blocks if buffer is empty; otherwise reserves one item.         |
| **Mutual exclusion**      | `wait(mutex)` / `signal(mutex)` | Only one process at a time can modify `buffer`, `in`, or `out`. |
| **Post-operation signal** | `signal(full)` (Producer)       | Announces a new item is available.                              |
|                           | `signal(empty)` (Consumer)      | Announces a slot has been freed.                                |

## Correctness Achieved

| Requirement          | Mechanism                                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Mutual Exclusion** | `mutex` protects critical sections.                                                                                |
| **No Overflow**      | Producer proceeds only when `empty > 0`.                                                                           |
| **No Underflow**     | Consumer proceeds only when `full  > 0`.                                                                           |
| **Progress**         | Atomic semaphore operations prevent deadlock; whichever process has its condition satisfied will continue.         |
| **Bounded Waiting**  | Each `wait` queues callers FIFO; each `signal` awakens one, ensuring finite wait time relative to buffer capacity. |

This semaphore-based solution allows the producer and consumer to run concurrently without race conditions, while respecting the bounded size of the shared buffer.

