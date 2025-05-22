---
date: '2025-05-22T05:22:08+05:00'
---
## shmget()
Allocates or accesses a shared memory segment using a key
```c
int shmget(key_t key, size_t size, int shmflg);
```
Return Value:
- Success: Shared memory segment ID
- Error: -1

## shmat()
Attaches a shared memory segment to the process's address space
```c
void *shmat(int shmid, const void *shmaddr, int shmflg);
```
Return Value:
- Success: Pointer to shared memory segment
- Error: -1

## shmdt()
Detaches a shared memory segment from the process's address space
```c
int shmdt(const void *shmaddr);
```
Return Value:
- Success: 0
- Error: -1

## shmctl()
Performs control operations on a shared memory segment (eg., remove, get info).
```c
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
Return Value:
- Success: 0
- Error: -1

## semget()
Creates a new semaphore set or accesses an existing one
```c
int semget(key_t key, int nsems, int semflg);
```
Return Value:
- Success: Semaphore set ID
- Error: -1

## semop()
Performs one or more atomic operations on semaphores
```c
int semop(int semid, struct sembuf *sops, size_t nsops);
```
Return Value:
- Success: 0
- Error: -1

## Memory Mapping
### mmap()
Maps a file or anonymous memory into the process's address space
```c
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
```
Return Value:
- Success: Pointer to mapped area
- Error: MAP_FAILED

### munmap()
Unmaps a previously mapped memory region from the address space
```c
int munmap(void *addr, size_t length);
```
Return Value:
- Success: 0
- Error: -1
