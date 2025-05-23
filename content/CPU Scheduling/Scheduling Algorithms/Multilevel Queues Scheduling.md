---
title: Multilevel Queues Scheduling
weight: 6
---
## Multilevel Queue Scheduling

**Definition**:
Multilevel Queue Scheduling divides the ready queue into multiple separate queues, each with its own priority level and scheduling algorithm. A process is permanently assigned to one of these queues based on characteristics like memory size, type (interactive/batch), or priority.

**Key Features**:

* **Queue Structure**: Multiple distinct queues (e.g., Foreground, Background)
* **Process Assignment**: Permanent to one queue
* **Scheduling Within Queue**: Each queue can have a different scheduling algorithm (e.g., RR for Foreground, FCFS for Background)
* **Scheduling Among Queues**:

  * **Fixed Priority Scheduling**: Always schedule from highest priority queue first
  * **Time Slicing Among Queues**: CPU time split across queues (e.g., 80% to Foreground, 20% to Background)
* **Drawback**: Processes in lower-priority queues can suffer starvation
* **Solution**: Time slicing across queues or **aging** within queues

