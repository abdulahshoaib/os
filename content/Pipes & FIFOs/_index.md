---
date: '2025-05-22T05:23:27+05:00'
draft: true
title: 'Pipes & FIFOs'
---
## Pipes & FIFOs
### pipe()
```
int pipe(int pipefd[2]);
```
Return Value:
- Success: 0
- Error: -1

### mkfifo()
```
int mkfifo(const char *pathname, mode_t mode);
```
Return Value:
- Success: 0
- Error: -1


