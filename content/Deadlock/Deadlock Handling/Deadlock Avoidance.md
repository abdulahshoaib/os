---
title: Deadlock Avoidance
weight: 2
---
Deadlock avoidance is a strategy that prevents deadlocks before they occur, by making resource allocation decisions dynamically based on a predicted system state. It ensures that the system will never enter an unsafe state, where deadlocks are possible.

## Key Requirements

1. **A Priori Knowledge**:
   Each process must declare in advance the maximum number of instances of each resource type it may ever request. This allows the system to make decisions using this knowledge.

2. **Resource Allocation State**:
   The system keeps track of:

   * **Available** resources (free instances of each resource type)
   * **Allocated** resources (resources currently assigned to processes)
   * **Maximum demand** (max resources a process might request)
   * **Need** = `Maximum - Allocated` (resources still needed to complete)

## Safe vs. Unsafe States

* A **safe state** is one in which there is a **sequence of processes** such that each can finish execution even if all of them request their maximum demand immediately.
* An **unsafe state** is not necessarily a deadlock, but **may lead** to one.

The **goal of deadlock avoidance** is to **never** let the system enter an **unsafe** state.

## Safety Algorithm

This simulates whether the system can proceed to termination from the current state:

1. Initialize:

   * `Work = Available`
   * `Finish[i] = false` for all i
2. Find a process `i` such that:

   * `Finish[i] == false`
   * `Need[i] ≤ Work`
3. If found:

   * `Work = Work + Allocation[i]`
   * `Finish[i] = true`
   * Repeat step 2
4. If all `Finish[i] = true`, then **state is safe**.

### Example

Let there be 3 processes and 3 resource types: A, B, C
Initial conditions:

* `Available = [3, 3, 2]`
* `Max = [[7, 5, 3], [3, 2, 2], [9, 0, 2]]`
* `Allocation = [[0, 1, 0], [2, 0, 0], [3, 0, 2]]`

When a process requests resources, simulate its effect. If the system remains **safe**, grant the request.

Let’s walk through the **Safety Algorithm** example step-by-step based on your given data to see if the system is in a **safe state**.

### Given Data

* Processes: P0, P1, P2

* Resource Types: A, B, C

* Available Resources:
  `Available = [3, 3, 2]`

* Max Matrix (Maximum resource needs per process):

  ```
  Max
    [7, 5, 3],   // P0
    [3, 2, 2],   // P1
    [9, 0, 2]    // P2
  ```

* Allocation Matrix (Resources currently allocated to each process):

  ```
  Allocation
    [0, 1, 0],   // P0
    [2, 0, 0],   // P1
    [3, 0, 2]    // P2
  ```

* Need Matrix (Max - Allocation)

  ```
  Need
    [7, 4, 3],   // P0
    [1, 2, 2],   // P1
    [6, 0, 0]    // P2
  ```

### Step 1: Initialization

* `Work = [3, 3, 2]` (Available)
* `Finish = [false, false, false]`

### Step 2: Check for a process whose `Need ≤ Work`

#### Check P0:

Need = \[7, 4, 3] > Work → Not eligible

#### Check P1:

Need = \[1, 2, 2] ≤ Work \[3, 3, 2] → Eligible

**Simulate P1 running and releasing resources**

* `Work = Work + Allocation[P1] = [3, 3, 2] + [2, 0, 0] = [5, 3, 2]`
* `Finish = [false, true, false]`

### Step 3: Repeat Step 2

#### Check P0:

Need = \[7, 4, 3] > Work → Not eligible

#### Check P2:

Need = \[6, 0, 0] > Work = \[5, 3, 2] → Not eligible

→ No progress possible right now

At this point, we cannot proceed. So we retry by checking again after more resources are possibly freed. But none are.

* `Finish = [false, true, false]`
* **Not all processes can finish**
* **System is NOT in a safe state**

## Banker's Algorithm

This is the most well-known deadlock avoidance algorithm, proposed by Edsger Dijkstra. It's called the **Banker's Algorithm** because it mimics a bank lending system:

* A process (customer) must request a loan (resource).
* The bank (OS) checks whether granting it leaves the system in a safe state.
* If yes, the resource is allocated.
* If not, the process must wait.

### Data Structures

Let:

* `n` = number of processes
* `m` = number of resource types

Matrices:

* `Available[1..m]`: Number of instances available for each resource type.
* `Max[n][m]`: Maximum demand of each process for each resource type.
* `Allocation[n][m]`: Current allocation to each process.
* `Need[n][m]` = `Max[n][m] - Allocation[n][m]`: Remaining needs of each process.

### Algorithm Steps

**When a process requests a resource**:

1. Check if request ≤ `Need`
2. Check if request ≤ `Available`
3. Temporarily allocate and simulate the result:

   * `Available = Available - Request`
   * `Allocation = Allocation + Request`
   * `Need = Need - Request`
4. Check if the resulting system is in a **safe state**

   * If **yes**, finalize the allocation.
   * If **no**, **roll back** the temporary allocation and make the process wait.
