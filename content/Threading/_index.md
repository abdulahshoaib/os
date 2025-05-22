---
date: '2025-05-22T05:22:42+05:00'
---
## pthread_create()
Creates a new thread and starts executing the specified routine in parallel
```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```
Return Value:
- Success: 0
- Error: Error number

## pthread_join()
Waits for the specified thread to terminate and optionally collects its return value
```c
int pthread_join(pthread_t thread, void **retval);
```
Return Value:
- Success: 0
- Error: Error number

## pthread_detach()
Marks a thread as detached so its resources are automatically released on termination
```c
int pthread_detach(pthread_t thread);
```
Return Value:
- Success: 0
- Error: Error number

## pthread_exit()
Terminates the calling thread and optionally returns a value to any joining thread
```c
void pthread_exit(void *retval);
```
Return Value:
- Does not return

## pthread_self()
Returns the thread ID of the calling thread
```c
pthread_t pthread_self(void);
```
Return Value:
- The ID of the calling thread

## pthread_cond_init()
Initializes a condition variable used for signaling between threads
```
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr);
```
Return Value:
- Success: 0
- Error: Error number

## pthread_cond_destroy()
Destroys a condition variable, freeing its resources
```
int pthread_cond_destroy(pthread_cond_t *cond);
```
Return Value:
- Success: 0
- Error: Error number
