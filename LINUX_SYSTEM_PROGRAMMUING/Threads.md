# THREADS
---

- Def: To run a multiple parallel tasks within a process, we use threads.
- Every process by default has single theread .i.e, main thread.
- The main thread starts execution from main function.
- when we are dealing with threads you must know about:

    1. Entry point
    2. Context info
    3. Stack area
    4. CPU time
- In programmer point of view every function has set of instructions enclosed within curly barces i.e, { }
- If any process is not completely executed by the CPU in a given time slice then control goes to the next process and the  phenomenon is called **premption**. 

    **Reasons for pre-emption:**

    1. When the cpu time expires. the cpu even goes to the next process.
    2. When we use blocking calls the control again goes to the next process.
    3. Whatever there is hardware interrupt. the process stops and control moves to the interrupt handler.
    4. When priority task is created, the control goes to it.
- In real time operating system every process is called task.
    ![](../images/Thread.png)

- When after 40ms, scanf() is used as then process-2 will be executed by CPU, but process-1 remains unutilized for 160ms.
- Inorder to effective utilize that remaing time we use threads.

    **Q)How do you create new thread?**
    
    - pthread_create is a library function which is used to create a thread. It is defined in libthread.so Internally every library function invokes system call and it invokes clone() system call. Hence clone() is a system call use for creating thread.

    **Note:**
    - From used point of view every function is multiple statements enclosed within {} baces, but from system point of view every function has instructions present in text/code segment.
    
    **Q)Why we are using pthread_create() insted of clone() for creating a thread?**
    - Because the arguments to clone() system call is complex and need alot of understanding.

    #### Compilation of a thread.
    ---
    - Compiler by default has only acccess to c-standard library functions. The compiler will not have any access to other libraries.

    ```
    like    libc.so
            libpthread.so
            libnt.so
            libm.so
            librt.so

        Default path for library function is /lib
    ```
    - Compiler can access to other library only when we provide some information about the compilation. The information passed is library name, The actual name used for above library is **"so nmae"** (Shared object name) is a just library name.
    - The name in between lib and so is called so name.
    - So, compilation **gcc -lpthread file.c** (or)  **gcc file.c -lpthread**. Here **'-l'** indicates the library.
