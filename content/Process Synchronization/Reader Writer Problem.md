---
breadcrumbs: true
title: Readers Writers Problem
weight: 6
---
## Readers Writers 1

### Problem Statement

Two kinds of concurrent processes share a common data set:

| Role       | Action                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------- |
| **Reader** | Needs **read-only** access; many readers may read simultaneously.                                   |
| **Writer** | Needs **exclusive** access; no other reader or writer may access the data while a writer is active. |

Constraints:

* Any number of **readers** may read **concurrently**.
* A **writer** must have **exclusive** access—no reader or other writer can access the data during a write.

### Solution (Readers-Preference)

#### Synchronization Primitives

| Semaphore | Initial Value | Meaning                                                                                            |
| --------- | ------------- | -------------------------------------------------------------------------------------------------- |
| `mutex`   | `1`           | Binary semaphore protecting the shared variable `readCount`.                                       |
| `wrt`     | `1`           | Binary semaphore granting **writers exclusive access** to the data; first reader also waits on it. |

`readCount` (integer) counts active readers.
All semaphore operations are atomic.

#### Shared Data

```c
int  readCount = 0;    // number of active readers
semaphore mutex = 1;   // guards readCount
semaphore wrt   = 1;   // writers (and first/last reader) lock
```

#### Reader

```c
while (true) {
    wait(mutex);               // protect readCount
        readCount++;
        if (readCount == 1)    // first reader locks writers out
            wait(wrt);
    signal(mutex);             // release readCount lock

    /* ----- critical section (read) ----- */
    read_data();
    /* ----------------------------------- */

    wait(mutex);               // protect readCount
        readCount--;
        if (readCount == 0)    // last reader lets writers in
            signal(wrt);
    signal(mutex);             // release readCount lock

    remainder_section();
}
```

#### Writer

```c
while (true) {
    wait(wrt);          // request exclusive access

    /* ----- critical section (write) ----- */
    write_data();
    /* ------------------------------------ */

    signal(wrt);        // release exclusive access

    remainder_section();
}
```

### How the Semaphores Coordinate the Processes

| Step                    | Semaphore Action                              | Effect                                                                  |
| ----------------------- | --------------------------------------------- | ----------------------------------------------------------------------- |
| **First reader entry**  | `wait(mutex)` → `readCount++` → `wait(wrt)`   | Locks out writers; subsequent readers proceed without waiting on `wrt`. |
| **Additional readers**  | `wait(mutex)` / `signal(mutex)`               | Only update `readCount`; no writer exclusion needed if `readCount > 0`. |
| **Last reader exit**    | `wait(mutex)` → `readCount--` → `signal(wrt)` | Re-enables writers when no reader remains.                              |
| **Writer entry / exit** | `wait(wrt)` / `signal(wrt)`                   | Provides writers with exclusive access; blocks readers when held.       |

### Correctness Achieved

| Requirement                | Mechanism                                                                                                |
| -------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Mutual Exclusion**       | `wrt` held by **either** a writer **or** the reader group (via first reader) ensures no overlap.         |
| **No Read–Write Conflict** | Writers cannot enter while `wrt` is held by readers; readers cannot start if a writer holds `wrt`.       |
| **Reader Concurrency**     | Multiple readers skip `wrt` once the first reader has locked it, allowing parallel reads.                |
| **Progress**               | If the data is free, whichever process (reader or writer) obtains the relevant semaphore first proceeds. |
| **Bounded Waiting**        | Readers waiting only contend for `mutex`; writers wait at most until current readers finish.             |

This semaphore arrangement (readers-preference variant) maximizes reader concurrency while still guaranteeing writers exclusive access when required.

## Readers Writers 2

This is the **second** classical formulation of the Readers-Writers problem — known as the **Writers-Preference** solution.

### Problem Statement

Two types of concurrent processes — **readers** and **writers** — share a **common resource** (usually a database or file):

| Role        | Goal                                                                  |
| ----------- | --------------------------------------------------------------------- |
| **Readers** | May access the resource **simultaneously** with other readers.        |
| **Writers** | Must have **exclusive access** — no other readers or writers allowed. |

### Solution (Writers-Preference)

#### Synchronization Primitives

| Semaphore | Initial Value | Purpose                                                              |
| --------- | ------------- | -------------------------------------------------------------------- |
| `mutex1`  | `1`           | Ensures mutual exclusion for `readCount`.                            |
| `mutex2`  | `1`           | Ensures mutual exclusion for `writeCount`.                           |
| `readTry` | `1`           | Allows writers to block new readers when they are waiting.           |
| `wrt`     | `1`           | Actual access control to the **shared resource** (held exclusively). |

#### Shared Data

```c
int readCount = 0;      // number of active readers
int writeCount = 0;     // number of waiting writers

semaphore mutex1 = 1;   // protects readCount
semaphore mutex2 = 1;   // protects writeCount
semaphore readTry = 1;  // blocks readers if writer is waiting
semaphore wrt = 1;      // resource access
```


#### Reader Process

```c
while (true) {
    wait(readTry);           // Check if writers are waiting
    wait(mutex1);
        readCount++;
        if (readCount == 1)  // First reader locks the resource
            wait(wrt);
    signal(mutex1);
    signal(readTry);

    // ----------- Critical Section (Read) -----------
    read_data();
    // -----------------------------------------------

    wait(mutex1);
        readCount--;
        if (readCount == 0)  // Last reader releases the resource
            signal(wrt);
    signal(mutex1);

    remainder_section();
}
```

#### Writer Process

```c
while (true) {
    wait(mutex2);
        writeCount++;
        if (writeCount == 1)   // First writer blocks new readers
            wait(readTry);
    signal(mutex2);

    wait(wrt);                // Lock the resource

    // ----------- Critical Section (Write) ----------
    write_data();
    // -----------------------------------------------

    signal(wrt);

    wait(mutex2);
        writeCount--;
        if (writeCount == 0)   // Last writer allows readers again
            signal(readTry);
    signal(mutex2);

    remainder_section();
}
```

### How the Semaphores Coordinate the Processes

| Step                            | Semaphore Mechanism                      | Effect                                                           |
| ------------------------------- | ---------------------------------------- | ---------------------------------------------------------------- |
| **Writers signal intent**       | First writer locks `readTry`             | Prevents **new readers** from entering if any writer is waiting. |
| **Multiple writers wait**       | All writers still wait on `wrt`          | Still enforces one-writer-at-a-time.                             |
| **First reader waits**          | Blocks on `readTry` if writer is pending | Enforces **writer preference**.                                  |
| **Last reader unlocks**         | `readCount == 0` → `signal(wrt)`         | Allows writers to proceed once all readers finish.               |
| **Last writer unlocks readers** | `writeCount == 0` → `signal(readTry)`    | New readers are now allowed to proceed.                          |

### Correctness Achieved

| Requirement            | Mechanism                                                                                     |
| ---------------------- | --------------------------------------------------------------------------------------------- |
| **Mutual Exclusion**   | `wrt` guarantees only one writer or many readers access the resource at any one time.         |
| **Reader Concurrency** | Multiple readers are allowed when no writer is waiting or active.                             |
| **Writer Preference**  | `readTry` blocks new readers once a writer is waiting, giving writers priority.               |
| **No Starvation**      | Writers get access once all active readers finish and new ones are blocked.                   |
| **Bounded Waiting**    | Writers waiting first will proceed first (FIFO on `wrt`), and readers are blocked until done. |

This version **avoids starvation of writers** by preventing new readers from entering the critical section once a writer has requested access. It ensures **fairness toward writers** even in a heavily read-oriented system.

