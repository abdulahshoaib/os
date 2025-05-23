---
breadcrumbs: true
title: Producer Consumer Problem
weight: 2
---
## Producer–Consumer Problem

**Definition**
Two cooperating processes—**Producer** and **Consumer**—share a buffer:

* **Producer** generates items and inserts them into the buffer.
* **Consumer** removes and uses those items.

## Race Condition

Without proper synchronization, concurrent updates to shared variables (e.g., `counter`) can interleave, producing incorrect results. The system must guarantee mutually exclusive access to the buffer and accurate tracking of its current occupancy.

## Buffer Types

| Buffer Type   | Capacity                | Producer Waits When   | Consumer Waits When |
| ------------- | ----------------------- | --------------------- | ------------------- |
| **Bounded**   | Fixed N                 | Buffer full           | Buffer empty        |
| **Unbounded** | Unlimited (practically) | ― (can always insert) | Buffer empty        |

In practice, most implementations use a bounded circular buffer because physical memory is finite.

Certainly.

## Busy-Waiting Implementation

### Shared Data

```c
#define BUFFER_SIZE 10
item buffer[BUFFER_SIZE];
int  in  = 0, out = 0, counter = 0;
```

* `buffer[BUFFER_SIZE]`: The circular buffer that holds items produced by the producer and consumed by the consumer.
* `in`: Index where the next item will be placed by the producer.
* `out`: Index where the next item will be removed by the consumer.
* `counter`: Tracks the number of items currently in the buffer.

### Producer

```c
while (1) {
    item nextProduced = produce_item();          // create data

    while (counter == BUFFER_SIZE);              // wait if buffer is full

    buffer[in] = nextProduced;                   // place item in buffer
    in = (in + 1) % BUFFER_SIZE;                 // move to next index
    counter++;                                   // increment item count
}
```

* `produce_item()`: Generates an item.
* The `while(counter == BUFFER_SIZE);` loop ensures the producer waits if the buffer is full.
* Once space is available, the item is inserted at the `in` index, `in` is updated modulo `BUFFER_SIZE` to wrap around circularly, and the `counter` is incremented.

### Consumer

```c
while (1) {
    while (counter == 0);                        // wait if buffer is empty

    item nextConsumed = buffer[out];             // take item from buffer
    out = (out + 1) % BUFFER_SIZE;               // move to next index
    counter--;                                   // decrement item count

    consume_item(nextConsumed);                  // use data
}
```

* The `while(counter == 0);` loop causes the consumer to wait if the buffer is empty.
* Once an item is available, it's taken from the `out` index, `out` is updated in a circular fashion, and `counter` is decremented.
* `consume_item()` consumes the retrieved item.

### Summary

* This implementation relies on a **shared circular buffer** and **busy-wait loops** for synchronization.
* It assumes the producer and consumer will not interfere with each other's updates to `counter`, `in`, and `out`.
* In practice, accessing shared variables like `counter` without proper synchronization leads to race conditions.

