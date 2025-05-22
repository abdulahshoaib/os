---
date: '2025-05-22T05:24:56+05:00'
---
## fork()
Creates a new child process by duplicating the calling (parent) process.
```c
pid_t fork(void);
```
Return Value:
- Parent: child's PID (positive)
- Child: 0
- Error: -1

## wait()
Suspends execution of the calling process until any of its child processes terminates.
```c
pid_t wait(int *status);
```
Return Value:
- Success: PID of terminated child
- Error: -1

## waitpid()
Waits for a specific child process or set of children to terminate.
```c
pid_t waitpid(pid_t pid, int *status, int options);
```
Return Value:
- Success: PID of child
- Error: -1
- With WNOHANG and no children ready: 0

## exit()
Terminates the calling process and performs cleanup (e.g., flushes stdio buffers).
```c
void exit(int status);
```
Return Value:
- Does not return

## _exit()
Immediately terminates the process without flushing stdio buffers or calling cleanup handlers.
```c
void _exit(int status);
```
Return Value:
- Does not return

## MACROS
### WIFEXITED
Returns true if the child terminated normally (via exit() or _exit()).
```c
WIFEXITED(status)
```
Return Value:
- True if child terminated normally

### WEXITSTATUS
Returns the exit status code of the child (only valid if WIFEXITED(status) is true).
```c
WEXITSTATUS(status)
```
Return Value:
- Return code when WIFEXITED is true

### WIFSIGNALED
Returns true if the child terminated due to an uncaught signal.
```c
WIFSIGNALED(status)
```
Return Value:
- True if child terminated by signal

### WTERMSIG
Returns the signal number that caused the child to terminate (only valid if WIFSIGNALED(status) is true).
```c
WTERMSIG(status)
```
Return Value:
- Signal number when WIFSIGNALED is true

### WIFSTOPPED
Returns true if the child process is currently stopped, not terminated.
```c
WIFSTOPPED(status)
```
Return Value:
- True if child is stopped

### WSTOPSIG
Returns the signal number that caused the child to stop (only valid if WIFSTOPPED(status) is true).
```c
WSTOPSIG(status)
```
Return Value:
- Signal number when WIFSTOPPED is true
