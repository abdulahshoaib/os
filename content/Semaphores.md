---
breadcrumbs: false
title: Semaphore
weight: 6
---
## Library Required
```c
#include <semaphore.h>
```

## Types
```c
sem_t       // unique identifier for a semaphore
```

## sem_open()
Opens or creates a named semaphore and returns a pointer to it.
```c
sem_t *sem_open(const char *name, int oflag, mode_t mode, unsigned int value);
```
**Return Value:**
* Success: Pointer to semaphore
* Error: SEM_FAILED

**Example:**
```c
sem_t *sem = sem_open("/mysem", O_CREAT | O_EXCL, 0644, 2);
```

## sem_close()
Closes a named semaphore descriptor without removing the semaphore.
```c
int sem_close(sem_t *sem);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
sem_close(sem);
```

## sem_unlink()
Removes a named semaphore from the system
```c
int sem_unlink(const char *name);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
sem_unlink("/mysem");
```

## sem_wait()
Decrements (locks) the semaphore, blocking if its value is zero.
```c
int sem_wait(sem_t *sem);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
sem_wait(sem);
```


## sem_trywait()
Tries to decrement the semaphore without blocking; fails if the value is zero.
```c
int sem_trywait(sem_t *sem);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
if (sem_trywait(sem) == -1) {
    // handle failure without blocking
}
```

## sem_post()
Increments (unlocks) the semaphore, potentially waking a waiting thread.
```c
int sem_post(sem_t *sem);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
sem_post(sem);
```

## sem_getvalue()
Retrieves the current value of the semaphore.
```c
int sem_getvalue(sem_t *sem, int *sval);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
int val;
sem_getvalue(sem, &val);
```

## sem_init()
Initializes an unnamed semaphore for use within a process or between processes.
```c
int sem_init(sem_t *sem, int pshared, unsigned int value);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
sem_t sem_local;
sem_init(&sem_local, 0, 1);
```

## sem_destroy()
Destroys an unnamed semaphore, freeing associated resources.
```c
int sem_destroy(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

**Example:**
```c
sem_destroy(&sem_local);
```


