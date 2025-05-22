---
date: '2025-05-22T05:24:42+05:00'
---
## execl()
Replaces current process with a new one using a path and a list of arguments
```c
int execl(const char *path, const char *arg, ..);
```
Return Value:
- Success: No return
- Error: -1

## execlp()
Like `execl()` but searches `PATH` for the executable
```c
int execlp(const char *file, const char *arg, ..);
```
Return Value:
- Success: No return
- Error: -1

## execle()
Like `execl()` but allows specifying a custom environment
```c
int execle(const char *path, const char *arg, .., char * const envp[]);
```
Return Value:
- Success: No return
- Error: -1

## execv()
Replaces current process using a path and an argument vector (array)
```c
int execv(const char *path, char *const argv[]);
```
Return Value:
- Success: No return
- Error: -1

## execvp()
Like `execv()` but searches `PATH` for the executable
```c
int execvp(const char *file, char *const argv[]);
```
Return Value:
- Success: No return
- Error: -1

## execvpe()
Like `execvp()` but also allows specifying a custom environment
```c
int execvpe(const char *file, char *const argv[], char *const envp[]);
```
Return Value:
- Success: No return
- Error: -1


