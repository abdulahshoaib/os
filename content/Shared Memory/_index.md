---
date: '2025-05-22T05:22:08+05:00'
draft: true
title: 'Shared Memory'
---
## Shared Memory
### shmget()
```
int shmget(key_t key, size_t size, int shmflg);
```
Return Value:
- Success: Shared memory segment ID
- Error: -1

### shmat()
```
void *shmat(int shmid, const void *shmaddr, int shmflg);
```
Return Value:
- Success: Pointer to shared memory segment
- Error: -1

### shmdt()
```
int shmdt(const void *shmaddr);
```
Return Value:
- Success: 0
- Error: -1

### shmctl()
```
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
Return Value:
- Success: 0
- Error: -1

### semget()
```
int semget(key_t key, int nsems, int semflg);
```
Return Value:
- Success: Semaphore set ID
- Error: -1

### semop()
```
int semop(int semid, struct sembuf *sops, size_t nsops);
```
Return Value:
- Success: 0
- Error: -1

## Memory Mapping
### mmap()
```
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
```
Return Value:
- Success: Pointer to mapped area
- Error: MAP_FAILED

### munmap()
```
int munmap(void *addr, size_t length);
```
Return Value:
- Success: 0
- Error: -1
