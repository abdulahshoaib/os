---
date: '2025-05-22T05:22:26+05:00'
draft: true
title: 'Semaphores'
---
## Semaphores
### sem_open()
```
sem_t *sem_open(const char *name, int oflag, mode_t mode, unsigned int value);
```
Return Value:
- Success: Pointer to semaphore
- Error: SEM_FAILED

### sem_close()
```
int sem_close(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

### sem_unlink()
```
int sem_unlink(const char *name);
```
Return Value:
- Success: 0
- Error: -1

### sem_wait()
```
int sem_wait(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

### sem_trywait()
```
int sem_trywait(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

### sem_post()
```
int sem_post(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1

### sem_getvalue()
```
int sem_getvalue(sem_t *sem, int *sval);
```
Return Value:
- Success: 0
- Error: -1

### sem_init()
```
int sem_init(sem_t *sem, int pshared, unsigned int value);
```
Return Value:
- Success: 0
- Error: -1

### sem_destroy()
```
int sem_destroy(sem_t *sem);
```
Return Value:
- Success: 0
- Error: -1
