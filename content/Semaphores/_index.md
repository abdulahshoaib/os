---
date: '2025-05-22T05:22:26+05:00'
---
## sem_open()
Opens or creates a named semaphore and returns a pointer to it.
```c
sem_t *sem_open(const char *name, int oflag, mode_t mode, unsigned int value);
```
Return Value:
- Success: Pointer to semaphore
- Error: SEM_FAILED

## sem_close()
Closes a named semaphore descriptor without removing the semaphore.
```c
int sem_close(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

## sem_unlink()
Removes a named semaphore from the system
```c
int sem_unlink(const char *name);
```
Return Value:
- Success: 0
- Error: -1

## sem_wait()
Decrements (locks) the semaphore, blocking if its value is zero.
```c
int sem_wait(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

## sem_trywait()
Tries to decrement the semaphore without blocking; fails if the value is zero.
```c
int sem_trywait(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

## sem_post()
Increments (unlocks) the semaphore, potentially waking a waiting thread.
```c
int sem_post(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

## sem_getvalue()
Retrieves the current value of the semaphore.
```c
int sem_getvalue(sem_t *sem, int *sval);
```
Return Value:
- Success: 0
- Error: -1

## sem_init()
Initializes an unnamed semaphore for use within a process or between processes.
```c
int sem_init(sem_t *sem, int pshared, unsigned int value);
```
Return Value:
- Success: 0
- Error: -1

## sem_destroy()
Destroys an unnamed semaphore, freeing associated resources.
```c
int sem_destroy(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1
