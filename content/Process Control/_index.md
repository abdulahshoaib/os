---
date: '2025-05-22T05:24:56+05:00'
---
## Library Used
```c
#include <sys/wait.h>
```

## fork()
Creates a new child process by duplicating the calling (parent) process.
```c
pid_t fork(void);
```
**Return Value:**
* Parent: child's PID (positive)
* Child: 0
* Error: -1

**Example:**
```c
pid_t pid = fork();
if (pid == 0) {
    printf("Child process\n");
    _exit(0);
} else {
    printf("Parent process, child PID: %d\n", pid);
}
```

## wait()
Suspends execution of the calling process until any of its child processes terminates.
```c
pid_t wait(int *status);
```
**Return Value:**
* Success: PID of terminated child
* Error: -1

**Example:**
```c
int status;
wait(&status);
```

## waitpid()
Waits for a specific child process or set of children to terminate.
```c
pid_t waitpid(pid_t pid, int *status, int options);
```
**Return Value:**
* Success: PID of child
* Error: -1
* With WNOHANG and no children ready: 0

**Example:**
```c
int status;
pid_t child = fork();
if (child == 0) _exit(5);
waitpid(child, &status, 0);
```

## exit()
Terminates the calling process and performs cleanup (e.g., flushes stdio buffers).
```c
void exit(int status);
```
**Return Value:**
- Does not return

**Example:**
```c
exit(0);
```

## _exit()
Immediately terminates the process without flushing stdio buffers or calling cleanup handlers.
```c
void _exit(int status);
```
**Return Value:**
- Does not return

**Example:**
```c
_exit(0);
```

## MACROS
### WIFEXITED
Returns true if the child terminated normally (via exit() or _exit()).
```c
WIFEXITED(status)
```
**Return Value:**
- True if child terminated normally

**Example:**
```c
int status;
wait(&status);
if (WIFEXITED(status)) {
    printf("Child exited normally\n");
}
```

### WEXITSTATUS
Returns the exit status code of the child (only valid if WIFEXITED(status) is true).
```c
WEXITSTATUS(status)
```
**Return Value:**
- Return code when WIFEXITED is true

**Example:**
```c
if (WIFEXITED(status)) {
    int code = WEXITSTATUS(status);
    printf("Exit code: %d\n", code);
}
```
### WIFSTOPPED
Returns true if the child process is currently stopped, not terminated.
```c
WIFSTOPPED(status)
```
**Return Value:**
- True if child is stopped

**Example:**
```c
if (WIFSTOPPED(status)) {
    printf("Child is stopped\n");
}
```

### WIFSIGNALED
Returns true if the child terminated due to an uncaught signal.
```c
WIFSIGNALED(status)
```
**Return Value:**
- True if child terminated by signal

**Example:**
```c
if (WIFSIGNALED(status)) {
    printf("Child killed by signal\n");
}
```
