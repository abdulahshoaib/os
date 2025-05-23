---
breadcrumbs: true
title: Critical Section Problem
weight: 3
---
## Problem Statement

When several cooperating processes access the **same shared data**, any segment of code that touches that data is called a **critical section**.
A correct solution must ensure:

| Requirement          | Meaning                                                                                                                                                             |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Mutual Exclusion** | At most **one** process can be inside its critical section at any instant.                                                                                          |
| **Progress**         | If no one is in a critical section, the decision of which process enters next cannot be postponed by processes that are only executing in their remainder sections. |
| **Bounded Waiting**  | After a process requests entry, there is a **finite bound** on how many times others may enter before it gets its turn.                                             |

Assumptions for classic software proofs: each process executes at a non-zero but unpredictable speed, and single‐word load/store instructions are **atomic**.

---

## Two-Process Software Algorithms

Let the processes be **P1** and **P1**.
`turn` is an integer (`0` or `1`); `flag[2]` is a Boolean array.

### Algorithm 1 – Strict Alternation

```c
// shared: int turn = 0;
// 0 → P1’s turn, 1 → P1’s turn

while (true) {
    while (turn != i) ;         // entry section (busy-wait)
    /* critical section */
    turn = j;                   // give turn to the other process
    /* remainder section */
}
```

| Property         | Result | Why?                                                                                             |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------ |
| Mutual Exclusion | ✅      | Only one `turn` value exists.                                                                    |
| Progress         | ❌      | If `turn == 0` and **P1** is ready while **P1** is in its remainder section, **P1** still spins. |
| Bounded Waiting  | ✅      | Processes alternate strictly.                                                                    |

### Algorithm 2 – Individual Flags

```c
// shared: bool flag[2] = {false,false};

// code for Pi (j = 1 - i)
while (true) {
    flag[i] = true;          // I want to enter
    while (flag[j]) ;        // wait while the other is ready
    /* critical section */
    flag[i] = false;         // I’m done
    /* remainder section */
}
```

| Property         | Result | Why?                                                                                                 |
| ---------------- | ------ | ---------------------------------------------------------------------------------------------------- |
| Mutual Exclusion | ✅      | A process enters only if `flag[j] == false`.                                                         |
| Progress         | ❌      | If **P1** sets `flag[0]=true` and, immediately after, **P1** sets `flag[1]=true`, both spin forever. |
| Bounded Waiting  | ❌      | Same scenario causes indefinite waiting.                                                             |

---

### Algorithm 3 – **Peterson’s Algorithm**

```c
// shared: bool flag[2] = {false,false};
          int  turn     = 0;        // whose turn if conflict

// code for Pi (j = 1 - i)
while (true) {
    flag[i] = true;                 // I want to enter
    turn    = j;                    // let the other go first
    while (flag[j] && turn == j) ;  // wait if j also wants in
    /* critical section */
    flag[i] = false;                // I’m done
    /* remainder section */
}
```

| Property         | Result | Reasoning (sketch)                                                                |
| ---------------- | ------ | --------------------------------------------------------------------------------- |
| Mutual Exclusion | ✅      | Both `flag[j]` **and** `turn==j` can’t be true for both processes simultaneously. |
| Progress         | ✅      | At most one process waits if both want to enter.                                  |
| Bounded Waiting  | ✅      | A waiting process can be bypassed only once before entering.                      |

---

## N-Process Solution – **Bakery Algorithm**

Every process takes a “ticket” like customers at a bakery.

### Shared Variables

```c
bool choosing[n] = {false};   // true while Pi is picking a number
int  number[n]   = {0};       // ticket numbers (0 ⇒ not competing)
```

### Pi’s Code

```c
while (true) {
    // entry section
    choosing[i] = true;                     // announce intention
    number[i]   = 1 + max(number[0..n-1]);  // take next ticket
    choosing[i] = false;

    fo*r (int j = 0; j < n; j++) {           // wait for turn
        while (choosing[j]) ;               // wait if Pj choosing
        while (number[j] != 0 &&
              (number[j] <  number[i] ||
              (number[j] == number[i] && j < i))) ;
    }

    /* critical section */

    number[i] = 0;                          // exit section
    /* remainder section */
}
```

### Why It Works

| Requirement      | Satisfied? | Explanation                                                                                           |
| ---------------- | ---------- | ----------------------------------------------------------------------------------------------------- |
| Mutual Exclusion | ✅          | Two processes cannot both see themselves “smaller” in the same lexicographic `(number, id)` ordering. |
| Progress         | ✅          | Tickets strictly increase; comparison uses only currently contending processes.                       |
| Bounded Waiting  | ✅          | Each process waits behind a **finite** set of smaller ticket numbers. New tickets are always larger.  |

The Bakery Algorithm generalizes Peterson’s idea to any number of processes without special hardware, guaranteeing a fair, starvation-free ordering of critical-section access.

Here's an **example problem** based on the **Bakery Algorithm** and the execution sequence you've described:

### Example Problem:

Five processes `P1` through `P4` are competing for entry into their critical sections using **Lamport’s Bakery Algorithm**.

At a particular moment, the following occurs:

1. All five processes begin executing their **entry sections** at approximately the same time.
2. `P1` does **not** intend to enter its critical section, and therefore **skips** taking a ticket (`number[1] = 0`).
3. All other processes (i.e., `P1`, `P2`, `P3`, `P4`) enter the entry section and begin executing the `choosing[i]` and `number[i]` assignment part.
4. The resulting ticket numbers are:

   * `P1` gets ticket **1**
   * `P2` gets ticket **3**
   * `P3` gets ticket **2**
   * `P4` gets ticket **4**

All processes now enter the second `for` loop of the Bakery Algorithm and start comparing their ticket numbers to others.

**Part A: Waiting Table (simplified version)**

| Process | Waiting for | Reason                                           |
| ------- | ----------- | ------------------------------------------------ |
| P1      | None        | Has lowest ticket number (1)                     |
| P2      | P1, P3      | P1 has lower ticket (1), P3 has lower ticket (2) |
| P3      | P1          | P1 has lower ticket (1)                          |
| P4      | P1, P3, P2  | All have lower ticket numbers (1, 2, 3)          |

**Part B: Execution Sequence**

1. `P1` enters critical section first.
2. `P1` exits CS and sets `number[0] = 0`.
3. Now `P3` has the smallest active number (2) → enters CS.
4. `P3` exits and resets `number[3] = 0`.
5. Now `P2` has the next smallest ticket (3) → enters CS.
6. `P2` exits and resets `number[2] = 0`.
7. `P4` now has the smallest active ticket (4) → enters CS.

Execution Order: <P1, P3, P2, P4>
