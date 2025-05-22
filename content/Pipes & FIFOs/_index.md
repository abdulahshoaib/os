---
date: '2025-05-22T05:23:27+05:00'
---
## pipe()
Creates a unidirectional data channel (pipe) using a pair of file descriptors for reading and writing
```c
int pipe(int pipefd[2]);
```
**Return Value:**
* Success: 0
* Error: -1

**Example:**
```c
int fd[2];
pipe(fd);
if (fork() == 0) {
    close(fd[0]);
    write(fd[1], "Hello", 5);
    _exit(0);
} else {
    close(fd[1]);
    char buf[6] = {0};
    read(fd[0], buf, 5);
    printf("Parent read: %s\n", buf);
}
```

## mkfifo()
Creates a named pipe (FIFO) special file that can be used for inter-process communication
```c
int mkfifo(const char *pathname, mode_t mode);
```
**Return Value:**
* Success: 0
* Error: -1

**Modes:**

| Octal | Symbolic    | Description                            |
| ----- | ----------- | -------------------------------------- |
| 0400  | r-- --- --- | Read by owner                          |
| 0200  | -w- --- --- | Write by owner                         |
| 0600  | rw- --- --- | Read/Write by owner                    |
| 0040  | --- r-- --- | Read by group                          |
| 0020  | --- -w- --- | Write by group                         |
| 0060  | --- rw- --- | Read/Write by group                    |
| 0004  | --- --- r-- | Read by others                         |
| 0002  | --- --- -w- | Write by others                        |
| 0006  | --- --- rw- | Read/Write by others                   |
| 0666  | rw- rw- rw- | Read/Write by all (owner/group/others) |
| 0644  | rw- r-- r-- | Owner RW, Group R, Others R            |

> [!TIP]
> - **Owner:** *The user who created the file and has primary control over it*
> - **Group:** *Set of different users*
> - **Others:** *Users neither owner nor group*


**Example:**
```c
mkfifo("mypipe", 0666);
int fd = open("mypipe", O_WRONLY);
write(fd, "Hi", 2);
```
