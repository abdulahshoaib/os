---
date: '2025-05-22T05:23:27+05:00'
---
## pipe()
Creates a unidirectional data channel (pipe) using a pair of file descriptors for reading and writing
```c
int pipe(int pipefd[2]);
```
Return Value:
- Success: 0
- Error: -1

## mkfifo()
Creates a named pipe (FIFO) special file that can be used for inter-process communication
```c
int mkfifo(const char *pathname, mode_t mode);
```
Return Value:
- Success: 0
- Error: -1


