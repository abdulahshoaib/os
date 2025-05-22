---
date: '2025-05-22T05:22:42+05:00'
draft: true
title: 'Threading'
---
### pthread_create()
## Threading
```
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```
Return Value:
- Success: 0
- Error: Error number

### pthread_join()
```
int pthread_join(pthread_t thread, void **retval);
```
Return Value:
- Success: 0
- Error: Error number

### pthread_detach()
```
int pthread_detach(pthread_t thread);
```
Return Value:
- Success: 0
- Error: Error number

### pthread_exit()
```
void pthread_exit(void *retval);
```
Return Value:
- Does not return

### pthread_self()
```
pthread_t pthread_self(void);
```
Return Value:
- The ID of the calling thread

### pthread_cond_init()
```
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr);
```
Return Value:
- Success: 0
- Error: Error number

### pthread_cond_destroy()
```
int pthread_cond_destroy(pthread_cond_t *cond);
```
Return Value:
- Success: 0
- Error: Error number

### pthread_cond_wait()
```
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
```
Return Value:
- Success: 0
- Error: Error number
