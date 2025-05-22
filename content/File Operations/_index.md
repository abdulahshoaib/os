---
date: '2025-05-22T05:24:21+05:00'
---
## Libraries Required
```c
#include <fcntl.h>
#include <unistd.h>
```

## Types
```c
size_t      // is a unsigned integer type used to represent the number of bytes read or written
ssize_t     // is a signed integer type used to represent the number of bytes read or written
FILE*       // pointer to struct that represent file stream
```

## open()
Opens a file and returns a file descriptor for it.
```c
int open(const char *pathname, int flags, mode_t mode);
```
**Retrun Value:**
* Success: File descriptor
* Error: -1

**Example:**
```c
int fd = open("example.txt", O_RDONLY);
```


## read()
Reads data from a file descriptor into a buffer
```c
ssize_t read(int fd, void *buf, size_t count);
```
**Retrun Value:**
* Success: Number of bytes read
* Error: -1

**Example:**
```c
char buf[128];
ssize_t nread = read(fd, buf, sizeof buf);
```

## write()
Writes data from a buffer to a file descriptor.
```c
ssize_t write(int fd, const void *buf, size_t count);
```
**Retrun Value:**
* Success: Number of bytes written
* Error: -1

**Example:**
```c
const char *msg = "Hello\n";
ssize_t nwritten = write(fd, msg, strlen(msg));
```

## close()
Closes an open file descriptor.
```c
int close(int fd);
```
**Retrun Value:**
* Success: 0
* Error: -1

**Example:**
```c
close(fd);
```

## printf()
Writes formatted output to standard output.
```c
int printf(const char *format, ...);
```

**Return Value:**
* Number of characters printed
* Negative value if an output error occurs

**Examples:**
```c
// Printing an integer
int x = 10;
printf("x = %d\n", x);

// Printing a float
float f = 3.14;
printf("f = %.2f\n", f);

// Printing a character
char c = 'A';
printf("Character: %c\n", c);

// Printing a string
char str[] = "Hello";
printf("String: %s\n", str);

// Printing multiple values
int id = 101;
char grade = 'A';
float marks = 89.5;
printf("ID: %d, Grade: %c, Marks: %.1f\n", id, grade, marks);

// Printing hexadecimal and octal
int num = 255;
printf("Hex: %x, Octal: %o\n", num, num);

// Printing a pointer address
int *ptr = &x;
printf("Address: %p\n", ptr);
```


## scanf()
Reads formatted input from standard input.
```c
int scanf(const char *format, ...);
```

**Return Value:**
* Number of items successfully read and assigned
* `EOF` if input failure occurs before any conversion

**Examples:**
```c
// Reading an integer
int x;
scanf("%d", &x);

// Reading a float
float f;
scanf("%f", &f);

// Reading a character
char c;
scanf(" %c", &c);  // space before %c to skip whitespace

// Reading a string
char str[100];
scanf("%s", str);  // stops at first whitespace

// Reading multiple values
int a, b;
scanf("%d %d", &a, &b);

// Reading a double
double d;
scanf("%lf", &d);

// Reading formatted values into different types
int id;
char grade;
float marks;
scanf("%d %c %f", &id, &grade, &marks);
```

## dup()
Duplicates an existing file descriptor to the lowest*numbered unused one.
```c
int dup(int oldfd);
```
**Retrun Value:**
* Success: New file descriptor
* Error: -1

**Example:**
```c
int dup_fd = dup(fd);       // dup_fd now refers to the same file
```


## dup2()
Duplicates a file descriptor to a specified descriptor number, closing it first if needed.
```c
int dup2(int oldfd, int newfd);
```
**Retrun Value:**
* Success: New file descriptor
* Error: -1

**Example:**
```c
dup2(fd, STDOUT_FILENO);    // redirect standard output to fd
```

## File Reading Functions
### fopen()
Opens a file and returns a file pointer to it.
```c
FILE *fopen(const char *filename, const char *mode);
```

**Return Value:**

* Success: Pointer to `FILE`
* Error: `NULL`

**Example:**
```c
FILE *fp = fopen("example.txt", "r");
```

### fclose()
Closes a previously opened file stream.

```c
int fclose(FILE *stream);
n
```

**Return Value:**

* Success: `0`
* Error: `EOF`

**Example:**
```c
fclose(fp);
```


### fgets()
Reads a string from a file, stopping at newline or EOF.

```c
char *fgets(char *str, int n, FILE *stream);
```

**Return Value:**

* Success: `str`
* Error or EOF: `NULL`

**Example:**
```c
char line[256];
fgets(line, sizeof line, fp);
```


### fgetc()
Reads a single character from a file.

```c
int fgetc(FILE *stream);
```

**Return Value:**

* Success: Character as an `unsigned char` cast to `int`
* Error or EOF: `EOF`

**Example:**
```c
int ch = fgetc(fp);     // returns int, not char
```

### fread()
Reads binary data from a file stream.

```c
size_t fread(void *ptr, size_t size, size_t count, FILE *stream);
```

**Return Value:**

* Number of full elements successfully read

**Example:**
```c
int block[512];
size_t n = fread(block, 1, sizeof block, fp);
```

### fscanf()
Reads formatted input from a file stream.

```c
int fscanf(FILE *stream, const char *format, ...);
```

**Return Value:**

* Number of items successfully read and assigned
* `EOF` if input failure occurs before any conversion

**Example:**
```c
int x, y;
fscanf(fp, "%d %d", &x, &y);
```


### feof()
Checks whether the end-of-file indicator is set.

```c
int feof(FILE *stream);
```

**Return Value:**

* Non-zero if EOF indicator is set
* `0` otherwise

**Example:**
```c
if (feof(fp)) { /* reached end-of-file */ }
```

### ferror()
Checks whether a file stream has an error.

```c
int ferror(FILE *stream);
```

**Return Value:**

* Non-zero if an error occurred
* `0` otherwise

**Example:**
```c
if (ferror(fp)) { /* handle stream error */ }
```

## File Writing Functions
### fprintf()
Writes formatted output to a file stream.

```c
int fprintf(FILE *stream, const char *format, ...);
```

**Return Value:**

* Number of characters printed
* Negative number on error

**Example:**
```c
fprintf(fp, "Value = %d\n", 42);
```

### fputs()
Writes a null-terminated string to a file.

```c
int fputs(const char *str, FILE *stream);
```

**Return Value:**

* Success: Non-negative value
* Error: `EOF`

**Example:**
```c
fputs("Line of text\n", fp);
```

### fputc()
Writes a single character to a file.

```c
int fputc(int c, FILE *stream);
```

**Return Value:**

* Success: Character written (as unsigned char cast to int)
* Error: `EOF`

**Example:**
```c
fputc('A', fp);
```

### fwrite()
Writes binary data to a file stream.

```c
size_t fwrite(const void *ptr, size_t size, size_t count, FILE *stream);
```

**Return Value:**

* Number of full elements successfully written

**Example:**
```c
fwrite(block, 1, sizeof block, fp);
```

### fflush()
Flushes a file’s output buffer to disk.

```c
int fflush(FILE *stream);
```

**Return Value:**

* Success: `0`
* Error: `EOF`

**Example:**
```c
fflush(fp);     // force buffered output to disk
```










