---
title: Clock
weight: 5
---
## What it does

It replaces the **first page found with a reference bit of 0** in a circular queue of memory frames.
This approximates **Least Recently Used (LRU)** behavior with much less overhead.

## How it works

* Each page in memory has a **reference bit** (R bit), initially `0`.
* When a page is **accessed**, its R bit is set to `1`.
* When a **page fault** occurs:

  * Move the **clock hand** forward.
  * For each page:

    * If R = `1`: set R = `0`, skip to next.
    * If R = `0`: **replace this page** with the new one.
* Insert the new page with R = `1`.

## Pros

* Efficient approximation of LRU.
* Does not require tracking exact usage history.
* Fast and simple to implement in hardware/software.

## Cons

* Slightly less accurate than true LRU.
* Still may evict frequently-used pages in edge cases.

## Example

Reference string: 0 4 1 4 2 4 3 4 2 4 0 4 1
Frames: 3

### Initial setup:

* All frames are empty.
* Clock hand starts at the first frame.
* All reference bits = `0` initially.
* When a page is inserted, its R bit is set to `1`.

| Step | Page | Frames  | R Bits    | Page Fault? |
| ---- | ---- | ------- | --------- | ----------- |
| 1    | 0    | 0 \_ \_ | \[0 0 0]  | ✅          |
| 2    | 4    | 0 4 \_  | \[0 0 0]  | ✅          |
| 3    | 1    | 0 4 1   | \[0 0 0]  | ✅          |
| 4    | 4    | 0 4 1   | \[0 1 0]  | ❌          |
| 5    | 2    | 2 4 1   | \[0 1 0]  | ✅          |
| 6    | 4    | 2 4 1   | \[0 1 0]  | ❌          |
| 7    | 3    | 2 4 3   | \[0 0 0]  | ✅          |
| 8    | 4    | 2 4 3   | \[0 1 0]  | ❌          |
| 9    | 2    | 2 4 3   | \[1 1 0]  | ❌          |
| 10   | 4    | 2 4 3   | \[1 1 0]  | ❌          |
| 11   | 0    | 2 4 0   | \[0 0 0]  | ✅          |
| 12   | 4    | 2 4 0   | \[0 1 0]  | ❌          |
| 13   | 1    | 1 4 0   | \[0 1 0]  | ✅          |

* Total Page Faults: 7
