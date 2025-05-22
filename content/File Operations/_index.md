---
date: '2025-05-22T05:24:21+05:00'
draft: true
title: 'File Operations'
---
## File Operations
### open()
```
int open(const char *pathname, int flags, mode_t mode);
```
Return Value:
- Success: File descriptor
- Error: -1

### read()
```
ssize_t read(int fd, void *buf, size_t count);
```
Return Value:
- Success: Number of bytes read
- Error: -1

### write()
```
ssize_t write(int fd, const void *buf, size_t count);
```
Return Value:
- Success: Number of bytes written
- Error: -1

### close()
```
int close(int fd);
```
Return Value:
- Success: 0
- Error: -1


### dup()
```
int dup(int oldfd);
```
Return Value:
- Success: New file descriptor
- Error: -1

### dup2()
```
int dup2(int oldfd, int newfd);
```
Return Value:
- Success: New file descriptor
- Error: -1


