---
title: Deadlock Recovery
next: Virtual Memory
weight: 4
---
### Deadlock Recovery

When a deadlock is detected (e.g., via detection algorithms), the system must recover to resume normal operation

Two main strategies:

## Process Termination

### Option 1: Abort All Deadlocked Processes

* Pros: Guarantees immediate recovery
* Cons: Wastes all computation done by processes → high cost

### Option 2: Abort One Process at a Time

* Kill one process at a time, re-run detection after each termination
* Repeat until the cycle (deadlock) is broken

### How to Choose Which Process to Abort?

This is called victim selection. We aim to minimize the total recovery cost

#### Factors to Consider:

| Factor                      | Description                                                   |
| --------------------------- | ------------------------------------------------------------- |
| Priority                | Is the process critical? (e.g., system vs user)               |
| Work Done               | How much has it already computed? Killing it wastes this work |
| Time to Finish          | Is it almost done? Better to let it finish than restart       |
| Resources Held          | More resources held → more likely to help resolve deadlock    |
| Resources Needed        | If a process still needs many more, it's riskier              |
| Interactive or Batch    | Killing interactive users harms UX more                       |
| # of processes affected | Is this process part of multiple deadlocks?                   |

> Goal: Choose the process whose termination frees resources but costs the least

## Rollback

Instead of terminating the process, we roll it back to an earlier safe state (i.e., a checkpoint).

### Rollback Strategy:

* Return the process to a known safe state
* Restart execution from that point

> Requires checkpoints to be taken periodically (e.g., using snapshotting or transaction logs)

### Starvation in Recovery

If the same process is always selected as the victim, it may never finish ⇒ starvation

### Solution:

Include the number of rollbacks in the cost metric
Processes that have already been rolled back many times should be less likely to be chosen again.

### Example Recovery Flow

1. Deadlock is detected involving P1, P2, P3
2. Choose victim (say P2 has:
   * low priority,
   * long remaining time,
   * holds lots of resources)
3. Kill P2
4. Release its resources
5. Re-run detection
6. If still deadlocked → choose another victim
7. Repeat until no cycles exist

### Summary Table

| Recovery Strategy   | Description                            | Trade-off                            |
| ------------------- | -------------------------------------- | ------------------------------------ |
| Abort all           | Kill all deadlocked processes          | Simple but expensive                 |
| Abort one by one    | Iteratively kill until deadlock breaks | Less loss, requires victim selection |
| Rollback            | Restart process from checkpoint        | Requires checkpointing overhead      |
| Starvation handling | Factor rollback count into victim cost | Fairer long-term system behavior     |
