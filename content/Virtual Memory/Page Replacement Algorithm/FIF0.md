---
title: FIFO
weight: 2
prev: "Page Replacement Algorithm"
---
## What it does

* Removes the page that has been in memory the **longest time**.
* Think of pages in memory as a **queue**: new pages go to the **end**, old ones removed from the **front**.

## How it works

* Maintain a **queue of pages in memory**.
* On page fault:

  * If there’s free space → insert the page.
  * Else → **remove the oldest** page (front of the queue), insert new one at end.

## Pros

* Simple to implement (just use a queue).

## Cons

* May remove heavily used pages.
* Suffers from **Belady’s Anomaly**: more frames can sometimes cause **more faults**!

## Example

Reference string: 7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2

Frames: 3

| Step | Page | Frames  | Page Fault?  |
| ---- | ---- | ------- | ------------ |
| 1    | 7    | 7 \_ \_ | ✅           |
| 2    | 0    | 7 0 \_  | ✅           |
| 3    | 1    | 7 0 1   | ✅           |
| 4    | 2    | 0 1 2   | ✅ (7 out)   |
| 5    | 0    | 0 1 2   | ❌           |
| 6    | 3    | 1 2 3   | ✅ (0 out)   |
| 7    | 0    | 2 3 0   | ✅ (1 out)   |
| 8    | 4    | 3 0 4   | ✅ (2 out)   |
| 9    | 2    | 0 4 2   | ✅ (3 out)   |
| 10   | 3    | 4 2 3   | ✅ (0 out)   |
| 11   | 0    | 2 3 0   | ✅ (4 out)   |
| 12   | 3    | 2 3 0   | ❌           |
| 13   | 2    | 2 3 0   | ❌           |

* Total Page Faults: 12


