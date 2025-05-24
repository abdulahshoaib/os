---
title: LRU
weight: 4
---
## What it does

 Replaces the page that has not been used for the longest time in the past.
 Assumes recently used pages will be used again soon.

## How it works

* Keep track of last-used time for each page.
* On page fault:

  * Replace the page that was **used least recently**.

Can be implemented using:

* Timestamps (expensive), or
* Stack / linked list (updated on every access)

## Pros

* Performs well in practice.
* No Belady’s anomaly.

## Cons

* More complex to implement.
* Slower than FIFO if done in software.

## Example

Same reference string: 7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2

Frames: 3

| Step | Page | Frames  | Page Fault?  | Explanation           |
| ---- | ---- | ------- | ------------ | --------------------- |
| 1    | 7    | 7 \_ \_ | ✅           | First use             |
| 2    | 0    | 7 0 \_  | ✅           | Add 0                 |
| 3    | 1    | 7 0 1   | ✅           | Add 1                 |
| 4    | 2    | 0 1 2   | ✅ (7 out)   | 7 least recently used |
| 5    | 0    | 0 1 2   | ❌           | Used recently         |
| 6    | 3    | 0 2 3   | ✅ (1 out)   | 1 LRU                 |
| 7    | 0    | 0 2 3   | ❌           | In memory             |
| 8    | 4    | 0 3 4   | ✅ (2 out)   | 2 LRU                 |
| 9    | 2    | 0 4 2   | ✅ (3 out)   | 3 LRU                 |
| 10   | 3    | 0 2 3   | ✅ (4 out)   | 4 LRU                 |
| 11   | 0    | 0 2 3   | ❌           | In memory             |
| 12   | 3    | 0 2 3   | ❌           | In memory             |
| 13   | 2    | 0 2 3   | ❌           | In memory             |

* Total Page Faults: 10
