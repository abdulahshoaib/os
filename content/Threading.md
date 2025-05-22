---
breadcrumbs: false
title: Threading
weight: 5
---
## Library Required
```c
#include <pthread.h>
```
## Types
```c
pthread_t       // uniquely identify a thread in POSIX thread programming
pthread_attr_t  // uniquely identify a thread attributes in POSIX
```

## pthread_create()
Creates a new thread and starts executing the specified routine in parallel
```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```
**Return Value:**
* Success: 0
* Error: Error number

**Example:**
```c
void* print_msg(void* arg) {
    printf("Thread: %s\n", (char*)arg);
    return NULL;
}

pthread_t tid;
pthread_create(&tid, NULL, print_msg, "Hello from thread");
```

## pthread_join()
Waits for the specified thread to terminate and optionally collects its return value
```c
int pthread_join(pthread_t thread, void **retval);
```
**Return Value:**
* Success: 0
* Error: Error number

**Example:**
```c
void* thread_func(void* arg) { return (void*)42; }

pthread_t tid;
pthread_create(&tid, NULL, thread_func, NULL);
void* retval;
pthread_join(tid, &retval);
printf("Thread returned: %ld\n", (long)retval);
```

## pthread_detach()
Marks a thread as detached so its resources are automatically released on termination
```c
int pthread_detach(pthread_t thread);
```
**Return Value:**
* Success: 0
* Error: Error number

**Example:**
```c
void* detached_func(void* arg) { pthread_exit(NULL); }

pthread_t tid;
pthread_create(&tid, NULL, detached_func, NULL);
pthread_detach(tid);
```

## pthread_exit()
Terminates the calling thread and optionally returns a value to any joining thread
```c
void pthread_exit(void *retval);
```
**Return Value:**
* Does not return

**Example:**
```c
void* thread_func(void* arg) {
    pthread_exit("Finished");
    return NULL;
}

pthread_t tid;
pthread_create(&tid, NULL, thread_func, NULL);
```

## pthread_self()
Returns the thread ID of the calling thread
```c
pthread_t pthread_self(void);
```
**Return Value:**
* The ID of the calling thread

**Example:**
```c
void* thread_func(void* arg) {
    printf("Thread ID: %lu\n", pthread_self());
    return NULL;
}

pthread_t tid;
pthread_create(&tid, NULL, thread_func, NULL);
```

## pthread\_attr\_init()

Initializes a thread attribute object with default values

```c
int pthread_attr_init(pthread_attr_t *attr);
```

**Return Value:**

* Success: 0
* Error: Error number

**Example:**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
```

## pthread\_attr\_destroy()

Destroys a thread attribute object and frees associated resources

```c
int pthread_attr_destroy(pthread_attr_t *attr);
```

**Return Value:**

* Success: 0
* Error: Error number

**Example:**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_destroy(&attr);
```

## Thread Attr Setter/Getter
These functions are used to set and get properties of a `pthread_attr_t` object **before** creating a thread.

### pthread\_attr\_setdetachstate / pthread\_attr\_getdetachstate

Sets or gets the detach state (joinable or detached).

```c
int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);
int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);
```

**Values:** `PTHREAD_CREATE_JOINABLE` (default) · `PTHREAD_CREATE_DETACHED`

**Example**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);

int state;
pthread_attr_getdetachstate(&attr, &state);   // state now holds the detach flag

pthread_attr_destroy(&attr);
```

### pthread\_attr\_setschedpolicy / pthread\_attr\_getschedpolicy

Sets or gets the scheduling policy.

```c
int pthread_attr_setschedpolicy(pthread_attr_t *attr, int policy);
int pthread_attr_getschedpolicy(const pthread_attr_t *attr, int *policy);
```

**Policies:** `SCHED_OTHER` (default) · `SCHED_FIFO` · `SCHED_RR`

**Example**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setschedpolicy(&attr, SCHED_FIFO);

int policy;
pthread_attr_getschedpolicy(&attr, &policy);  // policy now SCHED_FIFO

pthread_attr_destroy(&attr);
```

### pthread\_attr\_setschedparam / pthread\_attr\_getschedparam

Sets or gets scheduling parameters (e.g., priority).

```c
int pthread_attr_setschedparam(pthread_attr_t *attr,
                               const struct sched_param *param);
int pthread_attr_getschedparam(const pthread_attr_t *attr,
                               struct sched_param *param);
```

**Example**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);

struct sched_param sp = { .sched_priority = 20 };
pthread_attr_setschedparam(&attr, &sp);

struct sched_param out;
pthread_attr_getschedparam(&attr, &out);      // out.sched_priority == 20

pthread_attr_destroy(&attr);
```

### pthread\_attr\_setinheritsched / pthread\_attr\_getinheritsched

Sets or gets whether the thread inherits or explicitly uses scheduling attributes.

```c
int pthread_attr_setinheritsched(pthread_attr_t *attr, int inherit);
int pthread_attr_getinheritsched(const pthread_attr_t *attr, int *inherit);
```

**Values:** `PTHREAD_INHERIT_SCHED` · `PTHREAD_EXPLICIT_SCHED`

**Example**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setinheritsched(&attr, PTHREAD_EXPLICIT_SCHED);

int inherit;
pthread_attr_getinheritsched(&attr, &inherit); // inherit now explicit

pthread_attr_destroy(&attr);
```

### pthread\_attr\_setstacksize / pthread\_attr\_getstacksize

Sets or gets the thread stack size.

```c
int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);
int pthread_attr_getstacksize(const pthread_attr_t *attr, size_t *stacksize);
```

**Example**

```c
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setstacksize(&attr, 1024 * 1024);  // 1 MB

size_t sz;
pthread_attr_getstacksize(&attr, &sz);          // sz == 1048576

pthread_attr_destroy(&attr);
```

### pthread\_attr\_setstack / pthread\_attr\_getstack

Sets or gets both stack address and size.

```c
int pthread_attr_setstack(pthread_attr_t *attr,
                          void *stackaddr, size_t stacksize);
int pthread_attr_getstack(const pthread_attr_t *attr,
                          void **stackaddr, size_t *stacksize);
```

**Example**

```c
char stack_area[64 * 1024];                     // 64 KB buffer

pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setstack(&attr, stack_area, sizeof stack_area);

void *addr;
size_t sz;
pthread_attr_getstack(&attr, &addr, &sz);       // addr == stack_area, sz == 65536

pthread_attr_destroy(&attr);
```

