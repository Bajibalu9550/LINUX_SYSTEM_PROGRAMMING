# 1. What is meant by IPC Mechanism?

```c
1. IPC (Inter-Process Communication) is a mechanism that allows processes to communicate with each other and share data.
2. It is mainly used when multiple processes need to coordinate their actions or exchange information in an operating system.
```
# 2. Why we use IPC Mechanism? 
```c
1. Multiple processes did not communicate with eachother directly. They use IPC Mechanism to communicate between multiple process.

```
# 3. What are the types of IPC Mechanism's?  

```c
1. Pipe's
2. Named pipes
3. Message Queue
4. Shared memory
5. Semaphores

```
# 4. What is meant by “unicast” and “multicast” IPC?  

```c
Unicast IPC

Definition:
Unicast IPC means communication between one sender and one receiver process only.

In simple words:

One process sends a message → Only one specific process receives it.


Multicast IPC

Definition:
Multicast IPC means communication between one sender and multiple receivers simultaneously.

In simple words:

One process sends a message → Multiple processes receive it at the same time.
```
# 5. What is meant by PIPES?  

```c
1. It is an IPC Mechanism tha can be only used in related process(i.e, Parent and child process)
2. Pipes can be accessed via basic i/o calls or universal i/o calls.
```
# 6. What is meant by Blocking Calls? 

```c
1. A blocking call is a type of function or system call that does not return control to the program until the requested operation is completed.
2. In simple words — the process waits (is blocked) until the operation finishes.
```
# 7. What are the types of Blocking Calls?
```c
These blocking calls can be categorized based on what type of operation causes the block.

1. Blocking I/O Calls

These are the most common type of blocking calls.
They block a process while performing Input/Output (I/O) operations such as reading or writing.

Examples:

read() → waits until data is available to read

write() → waits until the data is written completely

accept() → waits until a client connects (in socket programming)

recv() and send() → wait for data transfer in network sockets

2. Blocking Synchronization Calls

These occur when a process waits for synchronization with another process or thread.
Used to prevent race conditions or ensure proper order of execution.

Examples:

wait() → waits for a child process to finish

pthread_join() → waits for a thread to complete

sem_wait() → waits for a semaphore to be available

pthread_mutex_lock() → waits if the mutex is already locked

3. Blocking System Calls (General)

These include any kernel-level call that waits for an event or resource to become available.
They are not limited to I/O or synchronization.

Examples:

sleep() → process blocks for a certain time

pause() → blocks until a signal is received

select() or poll() → block until one of multiple file descriptors becomes ready

nanosleep() → suspends execution for a specific duration

4. Blocking Communication Calls (IPC)

In Inter-Process Communication, blocking occurs when a process waits for data from another process.

Examples:

Pipes: read() blocks until data is written to pipe

Message Queues: msgrcv() blocks until a message is available

Sockets: recv() blocks until data arrives

Shared Memory: Often used with semaphores → blocked until allowed access


| Type of Blocking Call            | Description                                 | Example Functions                        |
| -------------------------------- | ------------------------------------------- | ---------------------------------------- |
| **Blocking I/O**                 | Waits for input/output to complete          | `read()`, `write()`, `accept()`          |
| **Blocking Synchronization**     | Waits for another process/thread/resource   | `wait()`, `sem_wait()`, `pthread_join()` |
| **Blocking System Call**         | Waits for a time/event/resource             | `sleep()`, `pause()`, `select()`         |
| **Blocking Communication (IPC)** | Waits for data/message from another process | `msgrcv()`, `recv()`, `pipe read()`      |


```

# 8. What are the different types of I/O Calls? 

```c
In an operating system, I/O (Input/Output) calls refer to system calls or functions that perform data transfer between a process and an I/O device (like a file, network socket, or pipe).

There are 4 main types of I/O operations based on how the process interacts with the kernel and whether it waits for the operation to finish.

1. Blocking I/O

Definition:
In blocking I/O, the process waits (is blocked) until the I/O operation (read/write) is completely finished.

Working:

Process requests data (e.g., read()).

Process is put to sleep until the data is ready.

Once data is available, it’s copied to user space.

Process resumes execution.

Example Code:

char buf[100];
int n = read(fd, buf, sizeof(buf)); // blocks until data arrives


Diagram:

Process:  |---request--->| (waiting...) |<---data ready---|


 Advantage: Simple and easy to implement.
 Disadvantage: Process cannot do anything else while waiting.

2. Non-Blocking I/O

Definition:
In non-blocking I/O, the process does not wait if the data is not ready.
The system call returns immediately — either with data or an error (like EAGAIN).

Working:

Process calls I/O function.

If data is not ready, call returns immediately.

Process can do other work and try again later.

Example Code:

fcntl(fd, F_SETFL, O_NONBLOCK); // Set file descriptor to non-blocking
int n = read(fd, buf, sizeof(buf));
if (n < 0)
    perror("No data available yet!");


 Advantage: CPU not wasted waiting.
 Disadvantage: Requires repeated polling (checking again and again).

3. I/O Multiplexing (a.k.a. Event-driven I/O)

Definition:
The process uses select(), poll(), or epoll() to wait for multiple I/O events simultaneously.
It only blocks when waiting for any one of several I/O sources to become ready.

Working:

Process registers multiple file descriptors.

Waits until at least one becomes ready.

Performs I/O only on ready descriptors.

Example Code:

fd_set readfds;
FD_ZERO(&readfds);
FD_SET(fd1, &readfds);
FD_SET(fd2, &readfds);

select(maxfd + 1, &readfds, NULL, NULL, NULL); // Blocks until one is ready


 Advantage: Efficient for handling multiple I/O sources (like servers).
 Disadvantage: Slightly more complex to code.

4. Asynchronous (AIO) I/O

Definition:
In asynchronous I/O, the process starts an I/O operation and continues executing without waiting.
When the I/O finishes, the kernel notifies the process (via signal or callback).

Working:

Process requests I/O.

Kernel performs I/O in background.

Process continues doing other work.

Kernel sends notification when done.

Example (Pseudo-code):

aio_read(&aiocb); 

 Advantage: Best performance; process never blocks.
Disadvantage: Most complex to implement and debug.

🔸 Comparison Table
| Type                       | Process Waits? | Description                           | Example Functions               | Use Case                     |
| -------------------------- | -------------- | ------------------------------------- | ------------------------------- | ---------------------------- |
| **Blocking I/O**           | Yes            | Process waits until I/O completes     | `read()`, `write()`             | Simple applications          |
| **Non-blocking I/O**       | No             | Returns immediately if data not ready | `fcntl(fd, O_NONBLOCK)`         | Real-time or polling systems |
| **I/O Multiplexing**       | Partial        | Waits for multiple I/O events         | `select()`, `poll()`, `epoll()` | Servers, event-driven apps   |
| **Asynchronous I/O (AIO)** | No             | Kernel notifies after completion      | `aio_read()`, `aio_write()`     | High-performance systems     |

```
# 9. What are the I/O calls we are used in IPC Mechanisms? 


In Inter-Process Communication (IPC), processes exchange data using various kernel-level mechanisms like pipes, FIFOs, message queues, shared memory, semaphores, and sockets.

Each of these mechanisms internally relies on I/O system calls provided by the operating system.

### 1. Pipes (Anonymous Pipes)

Used for: Communication between related processes (usually parent ↔ child).

Communication Type: Unidirectional (one process writes, another reads).

| Function  | Purpose                                                                 |
| --------- | ----------------------------------------------------------------------- |
| `pipe()`  | Creates a pipe and returns two file descriptors `[read_end, write_end]` |
| `read()`  | Reads data from the pipe (blocks if no data)                            |
| `write()` | Writes data into the pipe                                               |
| `close()` | Closes the unused end of the pipe                                       |


### Example:
```c
int fd[2];
pipe(fd);
write(fd[1], "Hello", 5);
read(fd[0], buffer, 5);
```
### 2. Named Pipes (FIFO)

Used for: Communication between unrelated processes.

Communication Type: Unidirectional or bidirectional.

I/O Calls Used:

| Function   | Purpose                                 |
| ---------- | --------------------------------------- |
| `mkfifo()` | Creates a named pipe (special file)     |
| `open()`   | Opens the FIFO file for reading/writing |
| `read()`   | Reads data from FIFO                    |
| `write()`  | Writes data to FIFO                     |
| `close()`  | Closes FIFO after use                   |


### Example:
```c
mkfifo("myfifo", 0666);
int fd = open("myfifo", O_WRONLY);
write(fd, "Hi", 2);
```
### 3. Message Queues

Used for: Exchange of messages between processes via kernel-managed queues.

Communication Type: Bidirectional (can send/receive).

I/O Calls Used:

| Function   | Purpose                                          |
| ---------- | ------------------------------------------------ |
| `msgget()` | Creates or accesses a message queue              |
| `msgsnd()` | Sends a message to the queue                     |
| `msgrcv()` | Receives a message from the queue                |
| `msgctl()` | Performs control operations (e.g., delete queue) |


### Example:
```c
msgget(key, IPC_CREAT | 0666);
msgsnd(msgid, &msg, sizeof(msg), 0);
msgrcv(msgid, &msg, sizeof(msg), 0, 0);
```
### 4. Shared Memory

Used for: Fastest way to share data between processes (no kernel copy needed).

Needs synchronization (via semaphores) to prevent race conditions.

I/O Calls Used:

| Function   | Purpose                                                 |
| ---------- | ------------------------------------------------------- |
| `shmget()` | Creates a shared memory segment                         |
| `shmat()`  | Attaches the shared memory to a process’s address space |
| `shmdt()`  | Detaches the shared memory segment                      |
| `shmctl()` | Controls or removes the shared memory segment           |


### Example:
```c
int shmid = shmget(key, 1024, IPC_CREAT | 0666);
char *data = (char*) shmat(shmid, NULL, 0);
strcpy(data, "Shared message");
shmdt(data);
```
### 5. Semaphores

Used for: Synchronization, not data transfer.

Helps coordinate access to shared resources.

I/O Calls Used:

| Function   | Purpose                                      |
| ---------- | -------------------------------------------- |
| `semget()` | Creates a semaphore set                      |
| `semop()`  | Performs semaphore operations (wait, signal) |
| `semctl()` | Controls or removes semaphore set            |


### Example:
```c
semget(key, 1, IPC_CREAT | 0666);
semop(semid, &operation, 1);
```
### 6. Signals

Used for: Sending asynchronous notifications between processes.

I/O Calls Used:

| Function                   | Purpose                             |
| -------------------------- | ----------------------------------- |
| `kill()`                   | Sends a signal to a process         |
| `signal()` / `sigaction()` | Defines how process handles signals |
| `pause()`                  | Waits for a signal                  |
| `alarm()`                  | Generates a signal after some time  |


### Example:
```c
signal(SIGINT, handler);
kill(pid, SIGINT);
```
### 7. Sockets

Used for: Communication between processes across same or different machines (network-based IPC).

Communication Type: Bidirectional (full-duplex).

I/O Calls Used:

| Function            | Purpose                              |
| ------------------- | ------------------------------------ |
| `socket()`          | Creates a socket                     |
| `bind()`            | Assigns address to socket            |
| `listen()`          | Waits for incoming connections       |
| `accept()`          | Accepts a connection (blocking call) |
| `connect()`         | Connects to a remote socket          |
| `send()` / `recv()` | Send or receive data                 |
| `close()`           | Closes the socket                    |


### Example:
```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
connect(sockfd, (struct sockaddr *)&server, sizeof(server));
send(sockfd, "Hello", 5, 0);
recv(sockfd, buffer, sizeof(buffer), 0);
```


### Summary Table

| IPC Mechanism         | Common I/O System Calls                                                       | Communication Type       |
| --------------------- | ----------------------------------------------------------------------------- | ------------------------ |
| **Pipe**              | `pipe()`, `read()`, `write()`, `close()`                                      | One-way (unidirectional) |
| **FIFO (Named Pipe)** | `mkfifo()`, `open()`, `read()`, `write()`, `close()`                          | One-way or two-way       |
| **Message Queue**     | `msgget()`, `msgsnd()`, `msgrcv()`, `msgctl()`                                | Two-way                  |
| **Shared Memory**     | `shmget()`, `shmat()`, `shmdt()`, `shmctl()`                                  | Two-way                  |
| **Semaphore**         | `semget()`, `semop()`, `semctl()`                                             | Synchronization only     |
| **Signal**            | `kill()`, `signal()`, `sigaction()`, `pause()`                                | One-way (asynchronous)   |
| **Socket**            | `socket()`, `bind()`, `listen()`, `accept()`, `connect()`, `send()`, `recv()` | Two-way                  |



# 10. What are the Blocking Calls used in IPC? 

### Definition:
Blocking calls in IPC are system calls that make a process wait until the requested communication or synchronization operation is completed.


### Common Blocking Calls in IPC Mechanisms
| IPC Mechanism       | Blocking Call | Description                                |
| ------------------- | ------------- | ------------------------------------------ |
| **Pipes / FIFOs**   | `read()`      | Waits until data is available to read      |
|                     | `write()`     | Waits if pipe/FIFO buffer is full          |
| **Message Queue**   | `msgrcv()`    | Waits until a message arrives in the queue |
|                     | `msgsnd()`    | Waits if the queue is full                 |
| **Shared Memory**   | `sem_wait()`  | Waits for access (synchronization)         |
| **Semaphore**       | `sem_wait()`  | Blocks until the semaphore is available    |
| **Sockets**         | `accept()`    | Waits for a client connection              |
|                     | `recv()`      | Waits until data is received               |
|                     | `send()`      | May block if send buffer is full           |
| **Signals**         | `pause()`     | Waits until a signal is received           |
| **Process Control** | `wait()`      | Blocks until a child process terminates    |



# 11. What is meant by Named Pipes? 

### Definition:
A Named Pipe, also called a FIFO (First In, First Out), is an Inter-Process Communication (IPC) mechanism that allows two or more unrelated processes to communicate with each other by reading and writing through a special file in the filesystem.


Key points

| Feature               | Description                                                              |
| --------------------- | ------------------------------------------------------------------------ |
| **Type**              | IPC mechanism                                                            |
| **Full Form**         | FIFO – First In, First Out                                               |
| **Communication**     | Unidirectional (one process writes, another reads)                       |
| **Between Processes** | Works for **unrelated** processes (unlike normal pipes)                  |
| **Data Flow**         | The data written first is read first (FIFO order)                        |
| **Exists as a File**  | Appears as a **special file** in the filesystem (created using `mkfifo`) |


# 12. Where is the FIFO Object created?  

```c
A FIFO (Named Pipe) object is created in the file system (directory structure) as a special file.

When you create a named pipe using the system call:

mkfifo("myfifo", 0666);


 A special file named “myfifo” is created in the current working directory (or the specified path).
This file is not a regular file — it’s a special FIFO file recognized by the kernel.

 Where exactly it exists

It exists in the file system namespace, like /home/user/myfifo or /tmp/myfifo.

It is managed by the kernel but represented as a file so that processes can open it using standard I/O calls (open(), read(), write()).
```
# 13. What is the call used to create a FIFO Object?  
```c
mkfifo("myfifo", 0666);
```
# 14. What are the Blocking Calls used in Named Pipes? 
```c
In Named Pipes (FIFOs), the main blocking system calls are:

1. open()

2. read()

3. write()
```
# 15. Why read system calls acts as a blocking call? 

```c
1. read() blocks because it waits for data to become available before returning to the process.
2. If no data is present, the process cannot proceed, so it is suspended (blocked) by the kernel until data arrives.

When a process calls:

int n = read(fd, buffer, size);


The kernel checks if there’s data available in the file descriptor fd.

If data is available → read() copies it to the buffer → returns immediately.

If no data is available → read() blocks the process and puts it in the sleep state until another process writes data.
```
# 16. Difference between the Named Pipes and Pipes?  

| Feature                 | Pipe (Anonymous Pipe)                                                                | Named Pipe (FIFO)                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| **Definition**          | A unidirectional communication channel between **related processes** (parent-child). | A special file in the filesystem used for communication between **unrelated processes**. |
| **Name in File System** | No name in file system (anonymous).                                                  | Has a **name in the filesystem** (created using `mkfifo()`).                             |
| **Communication**       | One-way (can be made two-way using two pipes).                                       | Usually one-way, but can be two-way using two FIFOs.                                     |
| **Process Relation**    | Works only between **related processes**.                                            | Works between **unrelated processes**.                                                   |
| **Lifetime**            | Exists **only while processes are running**.                                         | Exists **even if no process is using it**; persists in the filesystem until deleted.     |
| **Creation**            | `pipe(int fd[2])` system call.                                                       | `mkfifo("filename", 0666)` system call.                                                  |
| **Blocking Behavior**   | `read()` and `write()` are blocking if no data / buffer full.                        | Same — `open()`, `read()`, and `write()` can block.                                      |
| **Use Case**            | Parent-child communication.                                                          | Communication between unrelated processes or long-lived communication channels.          |

# 17. What is return value of read system call? 

| Return Value | Meaning                                                                                                |
| ------------ | ------------------------------------------------------------------------------------------------------ |
| **> 0**      | Number of **bytes actually read** and copied into the buffer. Can be **less than the requested size**. |
| **0**        | **End-of-File (EOF)** reached — no more data to read.                                                  |
| **-1**       | **Error occurred** — check `errno` for details.                                                        |


# 65. Write a program where multiple processes compete for access to a critical section using semaphores to ensure mutual exclusion.
```c
#include<stdio.h>
#include<semaphore.h>
#include<sys/wait.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        sem_t *sem;
        sem=sem_open("mysem",O_CREAT | O_EXCL,0640,1);
        if(sem == SEM_FAILED){
                perror("sem_open");
                exit(EXIT_FAILURE);
        }
        printf("Semaphores Created Successfully.\n");

        for(int i=0;i<3;i++){
                int pid=fork();
                if(pid<0){
                        perror("fork");
                        exit(EXIT_FAILURE);
                }
                else if(pid==0){
                        printf("Child %d waiting to enter critical section....\n",getpid());
                        sem_wait(sem);
                        printf("Child %d entered critical section.....\n",getpid());
                        sleep(2);
                        printf("Child %d leaving critical section.......\n",getpid());
                        sem_post(sem);
                        exit(0);
                }

        }
        printf("Parent %d waiting to enter critical section....\n",getpid());

        sem_wait(sem);
        printf("Parent %d entered critical section...\n",getpid());
        sleep(2);
        printf("Parent %d leving critical section.......\n",getpid());
        sem_post(sem);

        for(int i=0;i<3;i++){
                wait(NULL);
        }

        sem_close(sem);
        sem_unlink("mysem");

        printf("All process finished. semaphore removed.\n");
}

```
# 66.Write a C program to create a pipe and pass an array of integers from the parentprocess to the child process through the pipe.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        int fds[2];

        if(pipe(fds)==-1){
                perror("pipe");
                exit(EXIT_FAILURE);
        }

        int pid=fork();
        if(pid<0){
                perror("fork");
                exit(EXIT_FAILURE);
        }
        else if(pid > 0){
                close(fds[0]);
                write(1,"This is Parent process.\n",sizeof("This is Parent process.\n"));
                int n;
                printf("Enter array size: ");
                scanf("%d",&n);

                int a[n];
                printf("Enter array elements: ");
                for(int i=0;i<n;i++){
                        scanf("%d",&a[i]);
                }

                write(fds[1],&n,sizeof(int));
                write(fds[1],a,sizeof(a));

                close(fds[1]);
        }
        else if(pid==0){
                close(fds[1]);

                int n;
                read(fds[0],&n,sizeof(int));

                int arr[n];
                read(fds[0],arr,sizeof(arr));

                printf("Child process Received from Parent: ");

                for(int i=0;i<n;i++){
                        printf("%d ",arr[i]);

                }
                printf("\n");

                close(fds[0]);
        }
}

```
