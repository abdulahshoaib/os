---
breadcrumbs: false
title: Exec Family
weight: 2
---
## Library Required
```c
#include <unistd.h>
```

## execl()
Replaces current process with a new one using a path and a list of arguments
```c
int execl(const char *path, const char *arg, ..);
```
**Return Values:**
- Success: No return
- Error: -1

**Example:**
```c
execl("/bin/ls", "ls", "-l", NULL);
```

## execlp()
Like `execl()` but searches `PATH` for the executable
```c
int execlp(const char *file, const char *arg, ..);
```
**Return Values:**
- Success: No return
- Error: -1

**Example:**
```c
execlp("ls", "ls", "-l", NULL);
```

## execle()
Like `execl()` but allows specifying a custom environment
```c
int execle(const char *path, const char *arg, .., char * const envp[]);
```
**Return Values:**
- Success: No return
- Error: -1

**Example:**
```c
char *env[] = { "MYVAR=VALUE", NULL };
execle("/bin/ls", "ls", "-l", NULL, env);
```

## execv()
Replaces current process using a path and an argument vector (array)
```c
int execv(const char *path, char *const argv[]);
```
**Return Values:**
- Success: No return
- Error: -1

**Example:**
```c
char *args[] = { "ls", "-l", NULL };
execv("/bin/ls", args);
```

## execvp()
Like `execv()` but searches `PATH` for the executable
```c
int execvp(const char *file, char *const argv[]);
```
**Return Values:**
- Success: No return
- Error: -1

**Example:**
```c
char *args[] = { "ls", "-l", NULL };
execvp("ls", args);
```

## execvpe()
Like `execvp()` but also allows specifying a custom environment
```c
int execvpe(const char *file, char *const argv[], char *const envp[]);
```
**Return Values:**
- Success: No return
- Error: -1

**Example:**
```c
char *args[] = { "ls", "-l", NULL };
char *env[] = { "MYVAR=VALUE", NULL };
execvpe("ls", args, env);
```
