---
date: '2025-05-22T05:24:21+05:00'
---
## open()
Opens a file and returns a file descriptor for it.
```c
int open(const char *pathname, int flags, mode_t mode);
```
Return Value:
- Success: File descriptor
- Error: -1

## read()
Reads data from a file descriptor into a buffer
```c
ssize_t read(int fd, void *buf, size_t count);
```
Return Value:
- Success: Number of bytes read
- Error: -1

## write()
Writes data from a buffer to a file descriptor.
```c
ssize_t write(int fd, const void *buf, size_t count);
```
Return Value:
- Success: Number of bytes written
- Error: -1

## close()
Closes an open file descriptor.
```c
int close(int fd);
```
Return Value:
- Success: 0
- Error: -1


## dup()
Duplicates an existing file descriptor to the lowest-numbered unused one.
```c
int dup(int oldfd);
```
Return Value:
- Success: New file descriptor
- Error: -1

## dup2()
Duplicates a file descriptor to a specified descriptor number, closing it first if needed.
```
int dup2(int oldfd, int newfd);
```
Return Value:
- Success: New file descriptor
- Error: -1


