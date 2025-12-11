# 1. What is memory management in system programming? 
- Memory management is the process of allocating, organizing, and controlling a computer’s memory so that programs can run efficiently and safely without interfering with each other.
# 2. Define virtual memory. 
- Virtual memory is a technique that allows a computer to use hard disk space as extra RAM so programs can run even when physical memory is not enough.

# 3. Differentiate between physical memory and virtual memory.
| **Aspect**     | **Physical Memory**                                       | **Virtual Memory**                                                                                    |
| -------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Definition** | Actual RAM installed in the computer.                     | An abstraction of memory that gives each process its own address space.                               |
| **Location**   | Located in hardware (RAM chips).                          | Managed by the OS using RAM + disk (swap space).                                                      |
| **Size**       | Limited to the installed RAM.                             | Can be larger than physical memory, limited by disk space.                                            |
| **Access**     | Directly accessed by CPU.                                 | Accessed through page tables; translated to physical memory addresses by MMU.                         |
| **Purpose**    | Stores actual data and instructions of running processes. | Provides isolation and allows processes to run as if they have more memory than physically available. |
| **Speed**      | Faster (direct hardware access).                          | Slower than physical memory if swapping to disk occurs.                                               |
| **Management** | Managed by hardware (RAM chips).                          | Managed by the operating system using paging or segmentation.                                         |
| **Example**    | DDR4 RAM of 8 GB.                                         | Each process having a 4 GB virtual address space in a 1 GB RAM system.                                |


# 4. What is the role of an operating system in memory management? 
- Memory is a critical resource in a computer system. The Operating System (OS) is responsible for managing it efficiently to ensure smooth execution of multiple processes.
### 1. Allocation of Memory

- Assigns memory to processes when they start or request it.

- Provides stack, heap, and data segments for each process.

### 2. Tracking Memory Usage

- Keeps records of free and allocated memory blocks.

- Maintains tables or maps to prevent overlapping and illegal access.

### 3. Memory Protection

- Ensures one process cannot access memory of another process.

- Prevents crashes and data corruption.

- Achieved using MMU (Memory Management Unit) and page tables.

### 4. Virtual Memory Management

- Provides an illusion of large, continuous memory to each process.

- Maps virtual addresses to physical addresses.

- Uses paging or segmentation and swap space when RAM is insufficient.

### 5. Handling Memory Reclamation

- Frees memory when processes terminate or when blocks are no longer needed.

- Prevents memory leaks and fragmentation.

### 6. Optimization

- Uses algorithms like First-Fit, Best-Fit, Buddy System for efficient allocation.

- Swaps data in/out of RAM to maximize system performance.


# 86.Write a C program to demonstrate dynamic memory allocation using malloc().
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
    int n;
    printf("Enter size: ");
    scanf("%d",&n);
    
    int *ptr=(int *)malloc(sizeof(int) * n);
    if(ptr==NULL){
        printf("Memory allocation failed.\n");
        return 0;
    }
    
    printf("Memory allocated dynamically with %d Bytes.\n",n);
    
     printf("Enter elements: ");
    for(int i=-0;i<n;i++){
        scanf("%d",&ptr[i]);
    }
    
    printf("Array elements: ");
    for(int i=0;i<n;i++){
        printf("%d ",*(ptr+i));
    }
}

```
# 5. Explain the purpose of memory allocation. 
- Memory allocation is the process of reserving and managing memory for programs and data while a program is running. Its main purposes are:
  ChatGPT said:

Here’s a simple and clear explanation of the purpose of memory allocation, suitable for notes or exams:

Purpose of Memory Allocation

Memory allocation is the process of reserving and managing memory for programs and data while a program is running. Its main purposes are:

### 1. Store Program Data

- Provides memory for variables, arrays, structures, and objects.

- Ensures the program has space to store its working data.

### 2. Enable Dynamic Memory Use

- Allows programs to request memory at runtime depending on need.

- Supports flexible and efficient use of memory instead of fixed-size allocation.

### 3. Efficient Memory Management

- Allocates memory in a way that reduces waste.

- Helps in tracking free and used memory to avoid conflicts.

### 4. Support Multitasking

- Ensures each process gets its own memory space.

- Prevents processes from overwriting each other’s memory.

### 5. Improve Performance

- Proper memory allocation ensures programs run faster and efficiently.

- Reduces memory fragmentation and prevents memory leaks.


# 87. Implement a C program to allocate memory for an array dynamically using calloc().
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
    int n;
    printf("Enter size: ");
    scanf("%d",&n);
    
    int *ptr=(int *)calloc(n , sizeof(int) );
    if(ptr==NULL){
        printf("Memory allocation failed.\n");
        return 0;
    }
    
    printf("Memory allocated dynamically with %d Bytes.\n",n);
    
     printf("Enter elements: ");
    for(int i=-0;i<n;i++){
        scanf("%d",&ptr[i]);
    }
    
    printf("Array elements: ");
    for(int i=0;i<n;i++){
        printf("%d ",*(ptr+i));
    }
}
```
# 88. Write a C program to resize dynamically allocated memory using realloc().
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
    int n;
    printf("Enter size: ");
    scanf("%d",&n);
    
    int *ptr=(int *)calloc(n , sizeof(int) );
    if(ptr==NULL){
        printf("Memory allocation failed.\n");
        return 0;
    }
    
    printf("Memory allocated dynamically with %d Bytes.\n",n);
    
    
    int m;
    printf("Enter resize memory: ");
    scanf("%d",&m);
    ptr=realloc(ptr,m * sizeof(int));
    if(ptr==NULL){
        printf("Memory reallocation failed.\n");
        return 0;
    }
    
    printf("Memory reallocation successful\n");
    
    printf("Now the reallocated memory size: %d Bytes\n",m*sizeof(int));
    
}
```
# 89. Develop a program in C to allocate memory for a linked list node dynamically.
```c
#include<stdio.h>
#include<stdlib.h>

typedef struct node{
    int data;
    struct node *next;
}Node;

int main(){
    Node *temp=malloc(sizeof(Node));
    if(temp==NULL){
        printf("Memory allocation failed.\n");
        return 1;
    }
    
    printf("Memoru allocation for linkedlist node is successfull.\n");
    printf("Size of the node: %d\n",sizeof(Node));
}

```
