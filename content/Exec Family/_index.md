---
date: '2025-05-22T05:24:42+05:00'
---
### execl()
```
int execl(const char *path, const char *arg, ...);
```
Return Value:
- Success: No return
- Error: -1

### execlp()
```
int execlp(const char *file, const char *arg, ...);
```
Return Value:
- Success: No return
- Error: -1

### execle()
```
int execle(const char *path, const char *arg, ..., char * const envp[]);
```
Return Value:
- Success: No return
- Error: -1

### execv()
```
int execv(const char *path, char *const argv[]);
```
Return Value:
- Success: No return
- Error: -1

### execvp()
```
int execvp(const char *file, char *const argv[]);
```
Return Value:
- Success: No return
- Error: -1

### execvpe()
```
int execvpe(const char *file, char *const argv[], char *const envp[]);
```
Return Value:
- Success: No return
- Error: -1


