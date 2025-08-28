# Process Management Subsystem


Process Management Subsystem

## Process:   The Process is nothing but the program running in user space of RAM

gcc filename.c  a.out  execute

--> a.out file is stored in the hard-disk, when it is executed it get loaded into the RAM. The memory segment get loaded into the user space and the PCB of the program or process get loaded into the kernel.

                                                                  (OR)

--> Whenever a program comes into execution the contents of executable binary (a.out) copied from hard-disk to user space of RAM.

### The RAM is divided into two parts:

1. User Space of RAM --> Upper part of RAM

2. Kernel Space of RAM --> Lower part of RAM

-->Whenever the program loaded into RAM , Two more memory segments are added. Those are Heap & Stack Segments.

PCB: For every process we create a block of information in kernel space of RAM is known as Process Control Block or Process Descriptor


### PCB contains:


**1. Process Identifier(PID)**

**2. Parent Process Identifier(PPID)**

    Every Process is created from another process that another process is called Parent process.

**3. File Descriptor Table(FD table)**

      It contains information about the file that are being opened by the programmer. There are many files in kernel but it does not maintains all the information of these files.

**4. Signal Disposition Table/Signal Disposition/Disposition of Signal/Signal behaviour table**

When we are hanging in the infinite loop, then we press the key to come out of the loop
i.e.
Ctrl + C ⇒ Termination
Ctrl + Z ⇒ Suspend

These combinations when used in kernel space generate the signal.

Improper handling of pointers causes crash during runtime of execution and program crashes with segmentation fault. Then this process is aborted using signaling mechanism.

**5. Page Table**

For understanding requires virtual memory and further requires paging technique and address translation.

## Process: Memory segment in user space and PCB in kernel space collectively called as process.

Process = Memory segment in user space + PCB in kernel space

## Note:
There are many process in the system, every process have their own memoery segment and they have correspondig PCB in kernel space.

  text segment= Contains Machine instruction corresponding to compiled program.
  
  BSS segment=Block started by symbol used to store the uninitialized global vatiables.
  
  Data Segment= Data segment is used to store the initialized global variables and intitalized static local variables
  
![](./Screenshot%202025-08-20%20225316.png)

## Process Creation:

There are two ways to create a Process

1. By using commands
2. By using fork() System call

### **1. By using commands**

&rarr; From the executable file process can be created by using command __./a.out__ where a.out is the executable file obtained after compilation.

![](./Executable%20file.png)

### **2. By using fork() System calls**

### System call: 
It is used in user space to send the resuest to the sub-system in kernel space. Every system call has an equivalent call in kernal space which starts with **sys_**.

&rarr; fork() system call can be used to create a child process and the process from where the fork() is invoked is called the parent process.

&rarr; Once we call fork() control immediately jumps to kernel space invoking an equivalent function in kernel starting with sys_. ofrk() is actually sending request to kernel space subsystem.

        (or)

&rarr;When we call fork() we are sending request to process management subsystem present in kernel space.(i.e, Kernel space process management)

__NOTE : -__

A Process which invoke fork system call is called **parent proccess** and the new process get created is called **child process**.

![](./images/Child%20process.png)

&rarr;Thus Page table and fd table of parent process get copied to the child process.

&rarr; parent and child process are execute different block of code.

&rarr;Before fork() invokation there is only one process but after fork() invokation there are two process i.e, child process and parent process.

&rarr;By using the conditional statement after fork(), we ensure that parent process and child process will execute different block of codes. If conditional statements are not present then parent and child process will execute same block of code.


&rarr;To this conditional statement we pass return value of fork().

![](./images/fork()%20execution.png)

Form the above child process execution is not going to start from the main function() it start after the fork().

&rarr;__fork() returns twice once in parent process and once in child process__

- fork() returns pid of child process in parent process
- fork() return "0" in the child process.

## Orphan Process and Demon Process(Zombie Process)

- ## Orphan Process

  - If parent process terminates even before child process is called orphan process.
  
  ```  
   main(){
    id=fork();
    if(id==0){
      sleep(60);
    }
    else {
    sleep(30);
    }
  }
  ```

  &rarr; Once the main function terminates then the parent process will be terminated. The memory segment and PCB get loaded from user space and kernel space respectively.

  &rarr; The PPID of the orphan process is '1', it takes PID of the init process. Init process is the first process od OS boot.

  __Exit Vs return__

  &rarr; The returns takes return to the locatipn where function is invoked. Where as exit terminates the entire function.

  ![](./images/Exit%20vs%20return.png)

&rarr;
The kernel by default submit the status of child process to parent process and it is done by using command __wait(&stat)__.

&rarr; By using wait() we can get the exit code of child process in parent process.

&rarr; wait() is a __"blocking call"__.

&rarr; wait() comes out of the blocking state only when the child process get terminates.

  ![](./images/wait().png)

&rarr; Inside the child process wait() should not be executed so it is necessary to use exit().

__Note :__ When the exit is called then process is removed, i.e, removed means memory segment get eliminated and exit status get stored in PCB, after submitting the PCB exit status to parent PCB in kernel space, the child PCB get destroyed.

![](./images/Exitcode.png)

&rarr; If no wait() was there then we cannot get access to exit status of child process. Althrough the memory segments will be erased but PCB content will be then on calling exit(5);
&rarr;Using command ps -af we can be able to see the child process.
## Zombie process or Demon process

  - parent process dies but still has entry in the process table is called __Zombie process__ or defunt process or Demon process.

  - Process can be terminated by using the command "kill"   [kill -g pid of the process]
  
  - But Zombie process can be terminated by using "kill" command because memory segment of child process has be erased so no process because of no any instruction set.

    ![](./images/Exit%20status.png)

  - The exit code of the child process is passed to argument of wait() in parent process

  - To extract the exit code, which is present in second byte od variable 

  `````
                  WEXITSTATUS(stat);
  ```````` 

  &rarr; WEXITSTATUS(stat) is a predefined macro which extract 1 byte of exit code from the 4 byte variable.

## Fork() system call

  - Driver can never initiate I/O request. Application can initiate I/O request

  - One driver can access a single device if the devices are same in nature then one driver can access those I/O devices also. When more than one drivers interact woith single I/O devices there arises improper functioning.

  - We can not have multiple drivers accessing single device

  - we can have multiple applications sending request to single driver.

  - Application sends an request to drivers by using system call.

  - Hardware/Device can initiate a request by using __" Hardware interrupts"__

    ![](./images/request.png)

  __Note :__

  - There are specific group of system calls used to send request to driver from application those are called __"Basic I/O calls"__.

    ### Use of fork()
    
    1. To create multiple applications from single application.
    
    2. Shell program internally uses fork() + exec() family system call.

    3. In client and server progrmming we use fork() system call

    __Note :__

    - wait() returns the pid of the child process that haas been terminated.

                        pid=wait(&stat);

  - 1 byte exit code comming from child process. Remaining 3 bytes contain the information about normal/abnormal termination of the child process. In case of abnormal termination the signal used to terminate the specific child process is specified.

__Q) What are the variants of wait system call?__

  1. wait() : blocks until child process terminates.

  2. waitpid() : Basical;y it takes three arguments and it comes to unblocking state when any specific child process terminates.

__Note :__

  - vfork() is always combined with the exec() family of calls. to understand vfork() we need to understand the virtual memory which requires furthur concept of pageing technique and address tranlation.

__Q) Why do not we run progrm with in hard disk (or) why do we copy progrm to RAM for execurion.__

  1. Accesing a data present in hard-disk is slower mechanism compared to RAM

  2. Data can be accessed from hard-disk block-by-block, where as from RAM we can access byte-by-byte.

      ![](./images/progrm%20copy%20to%20ram.png)

  - Memory in CPU in the form of register and we use register name to refer to the CPU register.

   - Basic units of memory are bit and bytes.

  - RAM are divided into byte by byte location or RAM. Memory are splitted into byte by byte memory locations and we use pointer yo store the address of RAM

    - To copy the data from RAM to CPU we are using CPU instructions __"LDA"__(load);

    -  To copy the data from CPU to RAM we are using CPU instruction __"STR"__(store).
## Virtual Memory
- Suppose we have four process already loaded into RAM and we want to load one more process.

     ![](./images/Virtual%20memory.png)
  
  <u><b><big>Posibilities:</big></b></u>

    1. wait until one of the process terminates.
      
        - Actually we do not know the how much time takes a process to terminate.It depends upon processor. so not possible
    2. Terminates one of the process in between

        - For termination of process in between we need to have problem in the code. Again not possible
    3. Try to findout segment of these process which are not used longer duration of time

       - Yes, it can be done

    &rarr; In order to findout the segments which are not used for longer duration of in a process we need to follow some alogorithm.

    - Step-1: Hard disk is consist of two segments i.e, __"Swap area"__ in Linux and __"Banking storage"__ in windows and memory segments of process which are not used for longer duration we move to swap area of hard disk.

  __Note :__
     
     - It is not mendatory to have all segments in RAM for eecution. Some of the process segments may be there in swap area and still we can execute the process.

       ![](./images/Swap%20area%20of%20ahrd%20disk.png)
    - The empty spaces that has been generated are not continuous memory location. It is because some of the memory segment of process which does not last for longer duration of time has been copy to swap area of hard disk.

    - Since we have empty spaces load the new process, for that new process segments has to be splitted and load to nin-continuous or empty memory locations of RAM.
  
  __Note :__

    - To run a program, memory segments need not be continuous memory locations. It also can be uncontinuous memory locations.

    __Problems :__

    1. Keep track of segments of process copied to the swap area of hard disk.

    2. Also keep track of segments of new process which are present in uncontinuous memory locations.

  &rarr; To overcome these two problems we use __"Pagening Techniques"__.

# Pagening Technigues 

  - Here we dont the process directly into RAM. so before loading into RAM we divide the process into equal parts known as pages or virtual paage. Paages are numbered starting from zero.
  
  - The virtual memory scheme splits the memory used by the process into equal size parts called as pages or virtual pages.

  - size of each page can be 2KB,4KB,8KB,.... by default linux has page size of 4KB. In solaris os page size is of 8KB.

    ![](./images/Virtual%20pages.png)

  - RAM is also divided into equal size of parts called physical frames or page frames. Physical frame should be equal to that of page size. i.e, 4KB(default in linux).

  __Note :__

  - No of pages within the process are not fixed they changeds.
  
  __Q) why the no of pages are not fixed?__

  1. When heap and stack grows the number of pages with in process increases.

  2. When a new shared memory is created the number of pages increases.

  3. When we create a new block of memory using memory mapping call mmap().

  __Q) Why we require shared memory?__
  
  - By default multiple process can not share (i.e, send and receive) data. so shared memory is required to multiple process share the data.

  __Q) How Multiple process can share data?__

  - By using IPC(Inter process communication) mechnism.

    # IPC(Inter process communication)

    - Different tyoes of IPC mechanism are given below: 

      - Pipes
      - Named pipes(FIFO)
      - Message Queues
      - Shared memory
      - Semaphore 
  - Multiple Process must need IPC mechanism to share the data.
  - Physical frames are numbers starting from '0'
  - Initially the pages are stored in continously in physical frames of RAM. Over a period of time during the execution the pages goes into uncontinuos physical frames of RAM due to swap.

  __Page Table :__ 
  
  - Page table are present in PCB. Page number are used as indexed to this this page.
  - It is also called Page mapping info.
  - Page table contains the infomation about which pages are copied to which physical frames.

    ![](./images/Pagening%20techinve.png)

  - Each process have their own page table.
  - in above the page of process-1 and page of process -2 are cpoied into completely different physical frames.
  - Page table of process-1 and page tables of process-2 are pointing to completely different physical frames.

  __Q)Explain the scenario when pages aof process-1 and pages of process-2 are copied into same physical frame?__

                           (or)
__Q) Explain the senario where pages of multiplr process are sharing the same physical frames?__

                          (or)
__Q) Can the page table of multiple process point to same pgysical frames?__

- There are 3 scenatio 
  
  1. During child process creation. a parent process and child process pages are copied into same physical frames (or) Pages of parent process and child process are sharing same physical frames.

  2. When multiple process shares the data using shared memory IPC mechanism.

  3. When multiple process shares memory that is created using memory mapping call mmap().

  __1. Child Process Creation.__

  - Consider sample code 
  ```
  int x=10;

  main(){
    int stat;
    if=fork();

    if(id==0){
      x=20;
      printf("Child process\n");
      exit(1);
    }
    wait(&stat);
    printf("%d",WEXITSTATUS(stat));
    printf("%d",x);

  } 
  ```
- In the recent operating system the memory segment for child process is not created immediately. It iss created after some time of execution during initial stages of execution child process and parent process uses same memory segments.

  ![](./images/PArent%20and%20child%20same%20memory.png)

__Q)How child process uses the memory segment of parent process?__

- The memory segment of a process are divided into pages and these pages are accessed by both parent and child process.

- The page table of parent process and child process are same (or) the parent and child process are using the pages stored in same physical fraems. and sharing happens until the child process does READ operation. It is shown below: 

  ![](./images/Write%20on%20copy.png)

__Q) How Parent and child process keep track of which instruction they are going to execute?__

- They keep the infromation in the context area of PCB.

- Considering the sample program

  ```
  int x=10;
  main(){
    id=fork();
    printf("Child process");
    exit(5);
  }
  wait(&stat);
  printf("%d",WEXITSTATUS(stat));
  printf("%d",x);
  }
  ```

__Q) How parent and child process execute different block of code after fork()?__

- based on the return value of fork() being passed to conditional statement.

__Note :__

- Till read operation both parent and child shares same physical frames. once they perform WRITE operation physical frames does not ramain comman any more.

__Q) Explain write operation to pages shared to parent and child process?__

1. Child process is suspended.

2. Write-on-copy (or) copyu-on write technique 

    - Page -2 on which wrire operaion to be performed is duplicated.
    - Duplicated page-2 is copied into freely available physical frames

      - In below ,memory for x=20 is again created and page-2 is placed in freely avaliable 5th physical frame and page-2 corresponding entry in page table od child process is to be modified.
  3. Child Process Resumes.
  4. Write operation is performed on the 5th physical frame of RAM is updated.

      ![](./images/Write%20oon%20copy.png)  

  __Note :__

  - When parent and child process tries to do write operation write-on-copy (or) copy-on-write(COW) Technique is applied.
  - In case of vfork() write-on-copy technique is not applied but in fork() write-on-copy technique is applied.

# Address Translation

- Physical address vs virtual address

- Physical address is the address of physical memory locations of RAM.
- Virtual address contains the page number and offset.

  ![](../images/Address%20Translation.png)

  **Note:**
    - When ever we use '&'(i.e,ampercent), we get a address which is virtual address.
    - STR/LDR instruction is also operated on virtual address.
    - When CPU is executing this instruction, then virtual address is converted into physical address.
    ![](../images/CPU_to_mmu.png)

    - While CPU is executing an instruction it places address on address bus, data on data bus and write/read operation on control bus.

    **Q) What address CPU places on address bus?**
      
    - Virtual address
    
    ![](../images/CPU.png)
  
  - Since x is present somewhere in between page-2. if the memory x is present at the begining of x offset = 0

  **Q) When address are generated.?**
  - During Compilation C-statements are converted to assembly codes and from their to instruction thus, during compilation virtual address are generated.

  **Q) When Physical address are generated from virtual address?**
  - During run time or execution time physical address are generated from virtual address.

  **Note:**
  - CPU sends th evirtual address the mmu is going to extract page number from virtual address and it uses page number as index to the page table and get the corresponding frame number.
  ```
  Physical address of variable x=page number * Page size +offset
  ```

  - **ls -l** : list of all the files present in the recent directory. It is also called as Command/CLI(Command line interface)/Shell command/Tools

  **Q) Can we execute commands from C profram?**
  - For running commands from c-program there is c-standard library function. i.e, system(); it is used to execute shell commands from the c- program.
  - Also by using exec() family pf calls we can run commands from c-program

  **Note:**
  - vfork() is always used with exec() family of calls and to understand the exec() family of calls we need to understad
    1. Command line arguments
    2. Array of pointers
    3. Pointers

### Pointers

- When we dealing with  pointers must follow these three steps:

  - Creating a pointer. (Analysis about the data type)
  - Assigning the valid address
    
    - If invalid address is provided then runtime error or segmentation fault observed
  - Dereferencing the pointer

  - arr[0]=*(arr+0)=(0+arr)=0[arr] --> So,for accessing the array elemet we have 4 ways.

  ```
  int arr[5]={10,20,30,40,50};
  int *ptr;
  ptr=&arr[0];
  Then ptr[0]=*(ptr+0)=*(0+ptr)=0[ptr]=10;
  ptr[1]=*(ptr+1)=*(1+ptr)=1[ptr]=20;
  ptr[2]=*(ptr+2)=*(2+ptr)=2[ptr]=30;
  ptr[3]=*(ptr+3)=*(3+ptr)=3[ptr]=40;
  ptr[4]=*(ptr+4)=*(4+ptr)=4[ptr]=50;
  ```
  **Note:**
    - Pointer contains the address therefore sizeof int,char and float pointers are same.
  
  **Use of Pointers:**
    - Pointers are used to store the address and access the memory locations that are created statically or dynamically.
  - It is used to implement call by reference in functions.
  - Structure + Pointer = Data Structures

**Character pointer:**
  
  - By using character pointer we can store the base address of character, char array, returned by malloc and string.

  - We can store the string using char pointer and character array.

### Character array

  ```
  Char array[10]="RAMAN";
  ```
  - If the statement is declared globally = Data segment

  - If the statement is declared locally = Stack segment

### Character Pointer

```
Char *ptr="RAMAN";
```
  - Here pointer "ptr" will get separate 4 bytes memory and string will get separate 4 byte of memory.
  - String stored using pointers always get stored in "rodata" segment and the rodata segment is of read only type i.e, we cannot write anything into this segment.
  - Let us Consider an Example
  
    - char *ptr="Balu";  <= rodata segment
    
    printf("%c",ptr[3]);
    








    
    
