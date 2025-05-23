---
breadcrumbs: true
title: Sleeping Barber Problem
next: Deadlock
weight: 9
---
## Problem Statement

The **Sleeping Barber Problem** is a classic **inter-process communication and synchronization** problem. It models a barbershop with the following characteristics:

| Element          | Description                                                                         |
| ---------------- | ----------------------------------------------------------------------------------- |
| **Barber**       | Sleeps when there are no customers. Wakes up and serves a customer if there is one. |
| **Customers**    | Either get a haircut or leave if the waiting room is full.                          |
| **Waiting Room** | Has a limited number of chairs.                                                     |

### Key Conditions

## Solution

1. If there are no customers, the **barber sleeps**.
2. When a customer arrives:

   * If all chairs are **occupied**, the customer **leaves**.
   * If there is a **free chair**, the customer sits and **waits**.
3. When the barber finishes a haircut:

   * He checks for waiting customers.
   * If no one is waiting, he **sleeps again**.

### Synchronization Primitives

| Semaphore   | Initial Value | Purpose                                               |
| ----------- | ------------- | ----------------------------------------------------- |
| `customers` | 0             | Counts waiting customers (to wake up barber).         |
| `barbers`   | 0             | Number of barbers ready to cut hair.                  |
| `mutex`     | 1             | Ensures **mutual exclusion** for access to `waiting`. |

### Shared Variables

```c
int waiting = 0;           // number of customers waiting
int CHAIRS = N;            // total number of chairs

semaphore customers = 0;   // customers waiting
semaphore barbers = 0;     // barbers ready
semaphore mutex = 1;       // for mutual exclusion
```

### Customer Process

```c
Customer() {
    wait(mutex);                  // enter critical section
    if (waiting < CHAIRS) {
        waiting++;                // sit in a waiting chair
        signal(customers);        // notify barber
        signal(mutex);            // leave critical section

        wait(barbers);            // wait to be called by barber
        get_haircut();
    } else {
        signal(mutex);            // leave (no chair available)
        leave_shop();
    }
}
```

### Barber Process

```c
Barber() {
    while (true) {
        wait(customers);          // sleep if no customers
        wait(mutex);              // enter critical section

        waiting--;                // take a customer out of waiting
        signal(barbers);          // call customer
        signal(mutex);            // leave critical section

        cut_hair();               // perform haircut
    }
}
```

## How the Semaphores Coordinate the Processes

| Step                    | Semaphore Used      | Effect                                                 |
| ----------------------- | ------------------- | ------------------------------------------------------ |
| Customer arrives        | `wait(mutex)`       | Enters critical section to check space in waiting room |
| Free chair available    | `waiting++`         | Sits and waits                                         |
| Notify barber           | `signal(customers)` | Increments count of waiting customers                  |
| Barber sleeps           | `wait(customers)`   | Waits for a customer to arrive                         |
| Barber wakes up         | `signal(barbers)`   | Wakes up and allows customer to get haircut            |
| Protect waiting counter | `mutex`             | Ensures `waiting` is updated atomically                |

## Correctness Achieved

| Requirement          | Mechanism                                                                        |
| -------------------- | -------------------------------------------------------------------------------- |
| **Mutual Exclusion** | `mutex` protects access to `waiting` variable.                                   |
| **Deadlock-Free**    | Semaphores coordinate process entry without circular waits.                      |
| **No Starvation**    | FIFO nature of semaphore queues ensures fair access.                             |
| **Efficiency**       | Barber only sleeps when no customers; customers only wait if space is available. |

This model ensures that both customers and barbers behave correctly and efficiently with respect to the limited resources in the barbershop.

