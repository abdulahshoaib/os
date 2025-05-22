---
breadcrumbs: false
title: Shared Memory
weight: 7
---
## Libraries Required
```c
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <unistd.h>
```

## shmget()
Allocates or accesses a shared memory segment using a key
```c
int shmget(key_t key, size_t size, int shmflg);
```
**Return Value:**
* Success: Shared memory segment ID
* Error: -1

**Example:**
```c
int shmid = shmget(IPC_PRIVATE, 1024, IPC_CREAT | 0666);
```

### Options

| Option      | Description                                      |
| ----------- | ------------------------------------------------ |
| `IPC_CREAT` | Create the segment if it does not exist          |
| `IPC_EXCL`  | Fail if segment exists (used with `IPC_CREAT`)   |
| `0666`      | Read & write permissions for all (standard mode) |

## shmat()
Attaches a shared memory segment to the process's address space
```c
void *shmat(int shmid, const void *shmaddr, int shmflg);
```
**Return Value:**
* Success: Pointer to shared memory segment
* Error: -1

**Example:**
```c
char *data = (char *)shmat(shmid, NULL, 0);
```

## shmdt()
Detaches a shared memory segment from the process's address space
```c
int shmdt(const void *shmaddr);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
shmdt(data);
```

## shmctl()
Performs control operations on a shared memory segment (eg., remove, get info).
```c
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
Return Value:
- Success: 0
- Error: -1

**Example:**
```c
shmctl(shmid, IPC_RMID, NULL);
```
### Options

| Command    | Description                                       |
| ---------- | ------------------------------------------------- |
| `IPC_STAT` | Get current segment status into `shmid_ds` struct |
| `IPC_SET`  | Set permissions and other fields from `shmid_ds`  |
| `IPC_RMID` | Remove the segment                                |

## semget()
Creates a new semaphore set or accesses an existing one
```c
int semget(key_t key, int nsems, int semflg);
```
Return Value:
- Success: Semaphore set ID
- Error: -1

**Example:**
```c
int semid = semget(IPC_PRIVATE, 1, IPC_CREAT | 0666);
```

### Options

| Option      | Description                                   |
| ----------- | --------------------------------------------- |
| `IPC_CREAT` | Create the semaphore set if it does not exist |
| `IPC_EXCL`  | Fail if it exists (used with `IPC_CREAT`)     |
| `0666`      | Read & write permissions                      |

## semop()
Performs one or more atomic operations on semaphores
```c
int semop(int semid, struct sembuf *sops, size_t nsops);
```
Return Value:
- Success: 0
- Error: -1

**Example:**
```c
sops[0].sem_num = 0;
sops[0].sem_op = -1;
sops[0].sem_flg = 0;
semop(semid, sops, 1);
```

## Memory Mapping
### mmap()
Maps a file or anonymous memory into the process's address space
```c
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
```
Return Value:
- Success: Pointer to mapped area
- Error: MAP_FAILED

**Example:**
```c
int fd = open("file.txt", O_RDWR);
char *addr = mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
```

#### Options
| Flag / Protection | Description                                                |
| ----------------- | ---------------------------------------------------------- |
| `PROT_READ`       | Pages can be read                                          |
| `PROT_WRITE`      | Pages can be written                                       |
| `MAP_SHARED`      | Updates visible to other processes mapping same region     |
| `MAP_PRIVATE`     | Copy-on-write; changes not visible to other processes      |
| `MAP_ANONYMOUS`   | Mapping is not backed by any file (used with `-1` as `fd`) |
| `MAP_FAILED`      | Return value on error                                      |

### munmap()
Unmaps a previously mapped memory region from the address space
```c
int munmap(void *addr, size_t length);
```
Return Value:
- Success: 0
- Error: -1

**Example:**
```c
munmap(addr, 4096);
```
