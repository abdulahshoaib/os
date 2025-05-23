---
breadcrumbs: true
title: Dining Philosipher Problem
weight: 8
---
## Problem Statement

Five philosophers sit around a circular table. Each philosopher alternates between two states:

| State    | Action                                   |
| -------- | ---------------------------------------- |
| Thinking | Does not interact with forks.            |
| Eating   | Needs two forks (left and right) to eat. |

Each philosopher must pick up their **left** and **right** forks to eat. Each fork is shared between adjacent philosophers.

## Solution (Naive)

### Synchronization Primitives

| Semaphore | Initial Value | Purpose                                                                          |
| --------- | ------------- | -------------------------------------------------------------------------------- |
| `fork[i]` | `1`           | Represents availability of fork `i`. Only one philosopher can hold it at a time. |

### Shared Data

```c
#define N 5                        // number of philosophers

semaphore fork[N] = {1,1,1,1,1};  // one semaphore per fork
```

### Philosopher Process

```c
while (true) {
    think();                            // not accessing forks

    wait(fork[i]);                      // pick up left fork
    wait(fork[(i+1) % N]);              // pick up right fork

    eat();                              // critical section

    signal(fork[i]);                    // put down left fork
    signal(fork[(i+1) % N]);            // put down right fork
}
```

## How the Semaphores Coordinate the Processes

| Step              | Semaphore Action                              | Effect                                                    |
| ----------------- | --------------------------------------------- | --------------------------------------------------------- |
| **Acquire forks** | `wait(fork[i])` and `wait(fork[(i+1)%N])`     | Philosopher picks up both forks or blocks if unavailable. |
| **Eat section**   | No conflict                                   | Exclusive access to both forks ensures mutual exclusion.  |
| **Release forks** | `signal(fork[i])` and `signal(fork[(i+1)%N])` | Makes forks available to neighbors.                       |

## Deadlock

If all philosophers pick up their left forks simultaneously, no one can pick up their right fork — resulting in a **deadlock**.

| Condition        | Present |
| ---------------- | ------- |
| Mutual Exclusion | Yes     |
| Hold and Wait    | Yes     |
| No Preemption    | Yes     |
| Circular Wait    | Yes     |

All four Coffman conditions for deadlock are present.

## Correctness Issues

| Requirement          | Problem in Naive Solution                       |
| -------------------- | ----------------------------------------------- |
| **Mutual Exclusion** | Satisfied via binary semaphores on forks.       |
| **Deadlock-Free**    | ❌ May deadlock if all pick left fork first.     |
| **No Starvation**    | ❌ If a philosopher is always blocked by others. |
| **Concurrency**      | ✅ When forks are free, philosophers eat freely. |

## Solution (Improved)

To avoid deadlock and ensure progress and bounded waiting, the solution must prevent all four philosophers from holding one fork and waiting for the other.

Use a single mutex semaphore for mutual exclusion and N state flags to track each philosopher’s status. Use condition semaphores for each philosopher to avoid busy waiting.

### Synchronization Primitives

| State    | Meaning                            |
| -------- | ---------------------------------- |
| THINKING | Not interested in forks            |
| HUNGRY   | Wants to eat (tries to pick forks) |
| EATING   | Holding both forks                 |

### Shared Data

```c
#define N 5

enum { THINKING, HUNGRY, EATING } state[N]; // philosopher states

semaphore mutex = 1;                        // global mutex
semaphore S[N];                             // one semaphore per philosopher
```

### `test(i)`

Check if philosopher `i` can eat (both neighbors are not eating).

```c
void test(int i) {
    if (state[i] == HUNGRY &&
        state[(i+N-1)%N] != EATING &&
        state[(i+1)%N] != EATING) {

        state[i] = EATING;
        signal(S[i]);  // wake up philosopher i
    }
}
```

### `take_forks(i)`

Philosopher `i` wants to eat.

```c
void take_forks(int i) {
    wait(mutex);              // enter critical section
        state[i] = HUNGRY;
        test(i);              // check if i can eat
    signal(mutex);            // leave critical section

    wait(S[i]);               // block if not yet able to eat
}
```

### `put_forks(i)`

Philosopher `i` finishes eating and releases forks.

```c
void put_forks(int i) {
    wait(mutex);              // enter critical section
        state[i] = THINKING;
        test((i+N-1)%N);      // check if left neighbor can now eat
        test((i+1)%N);        // check if right neighbor can now eat
    signal(mutex);            // leave critical section
}
```

### Philosopher Code

```c
while (true) {
    think();                  // not interested in forks

    take_forks(i);            // try to acquire forks

    eat();                    // critical section

    put_forks(i);             // release forks
}
```

## How the Semaphores Coordinate the Processes

| Issue                | Resolution                                                              |
| -------------------- | ----------------------------------------------------------------------- |
| **Deadlock**         | Avoided: a philosopher only proceeds when both forks are available.     |
| **Starvation**       | Prevented by checking neighbors after every `put_forks` (fair wakeups). |
| **Mutual Exclusion** | Guaranteed: Only one philosopher accesses shared state at a time.       |
| **Concurrency**      | Enabled: Non-adjacent philosophers may eat simultaneously.              |

## Correctness Achieved

| Requirement          | Mechanism                                                                   |
| -------------------- | --------------------------------------------------------------------------- |
| **Mutual Exclusion** | Global `mutex` ensures exclusive access to shared state.                    |
| **Deadlock-Free**    | `test()` only allows eating if neighbors are not eating.                    |
| **No Starvation**    | Each philosopher eventually gets a chance to eat via FIFO semaphore `S[i]`. |
| **Concurrency**      | Multiple non-neighbor philosophers can eat simultaneously.                  |
