---
title: OPT
weight: 3
---
## What it does

Replace the page that will not be used for the longest time in the future
This gives the minimum number of page faults possible

## How it works

* On page fault, look ahead in the reference string.
* Remove the page that will not be needed for the longest time

## Pros

* Best possible performance
* Used as a benchmark

## Cons

Not implementable in practice (you can’t predict future references).

## Example

Same reference string: 7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2

Frames: 3

| Step | Page | Frames  | Page Fault?  | Explanation                              |
| ---- | ---- | ------- | ------------ | ---------------------------------------- |
| 1    | 7    | 7 \_ \_ | ✅           | Load 7                                   |
| 2    | 0    | 7 0 \_  | ✅           | Load 0                                   |
| 3    | 1    | 7 0 1   | ✅           | Load 1                                   |
| 4    | 2    | 0 1 2   | ✅ (7 out)   | 7 used never again                       |
| 5    | 0    | 0 1 2   | ❌           | Already in                               |
| 6    | 3    | 0 2 3   | ✅ (1 out)   | 1 used far later                         |
| 7    | 0    | 0 2 3   | ❌           | In memory                                |
| 8    | 4    | 0 3 4   | ✅ (2 out)   | 2 used soon, but 2 is best to evict here |
| 9    | 2    | 0 3 2   | ✅ (4 out)   | 4 used never again                       |
| 10   | 3    | 0 3 2   | ❌           | In memory                                |
| 11   | 0    | 0 3 2   | ❌           | In memory                                |
| 12   | 3    | 0 3 2   | ❌           | In memory                                |
| 13   | 2    | 0 3 2   | ❌           | In memory                                |

* Total Page Faults: 9
