# File Management Subsystem

**Need:** 
- To access file present in physical memory or virtual memory

- There are three types of files present in Linux os.

    1. Normal files
    2. Special files
    3. Device files

## 1. Normal Files

- There are two types of normal files

    **a)** Texttual files
    
    **b)** Binary Files

#### a) Textual Files
----
- For accessing textual files we have text editors. Text editors are two types:
 i) CLI Text editors(vi, vim) ii) GUI base Text editors (gedit, editplus)

 - Textual files are saved with extension of  .c, .txt, .s, etc.....

#### b) Binary Files
---
- Binary files are saved with extension of .o, .out, ls, ps, mp3, mp4, .jpg 
- For accessing binary files we have separate applications for each of these

     .o, a.out  &rarr; objdump

     .mp3 &rarr; mp3 player

     .jpg &rarr; image viewer

## 2. Special Files

- Contains IPC objects, named pipes and FIFO's multiple process cannot directly shares data they need IPC mechanism

## 3. Device Files

- In linux user space, every device file is seen as device nodes. They are present in /dev directory. Device files are also called as device nodes.


**Q) How User space application sends request to hardware?**

- An application can never send request directly to hardware. It uses system call (i.e, basic i/o calls) to interact with the drivers. 

**Q) How do you make sure that only request to particular driveer is sent?**

- Every drivers has unique device files and the application has to use basic i/o calls on device file to send request to particular driver and particular driver initiates the corresponding hardware. This concept is applicable fpr windows, android, etc.. as they all have user space as well as kernel space.

    #### Serial port
    ---
    - Serial ports are needed for the serial communication.
    - Serial port uses the device file **tts0** present in **/dev** directory.

    #### Parallel Port
    ---
    - Parallel ports are used in printers.

    - Parallel port &rarr; /dev/parport

    #### Hard disk
    ---
    - Hard disk &rarr; /dev/sda       (sda0, sda1)
            
        - /dev/sab &rarr; 2nd hard disk
        - /dev/sac &rarr; 3rd hard disk
    
    **Note:**
    - Every device will have corresponding device file in /dev directory. Whenever we think about anu device immediately look for the corresponding file in /dev directory.

    - Basic i/o calls are called universal i/o calls because it is used to access the normal files, special files and device files.

    - Basic i/o calls are:   open(),  close(), read(), write(), ioctl(), fcntl().

    **Note:**   We cannot perform read and write operation until unless we open the file

    ## Open() System Call
    ```
    Syntax: open(Name of file, Modes, Permission in octal form);


    Syntax: open("file.txt", O_RDWR, 064);

    ```
    - Permissions are needed when we are creating the file.
    - Integer values can be expressed in:
        
        1. Decimal &rarr; Used for general calculation.
        2. Octal &rarr; For applying permission to files of various system resourses
        3. Hexadecimal &rarr; For address (Physical/Virtual).
    - fork(), exec() family calls send the request to the process management subsystem in kernel space
    - Basic i/o calls send request to the file management sub-system in kernel space.

        ![](../images/Basic%20io%20calls.png)

    - open() calls invokes the sys_open() which sends request to the driver to search the file name "file.txt", in the hard disk. The kernel is going to search the file name file.txt

    - Device Drivers are classified into 3 types
        
        1. Character device driver
        2. Block device driver
        3. Network device driver.

    - Character device uses character drivers and apporoximate 80% devices are character devices.

    - Block device uses block drivers e.g, Hard disk, USB, Pendrive. It is also called as Mass Storage Device.

    - Network device uses network drivers. eg, LAN port. The alternative name of network device is NIC (Network Interference Calls).

    -  When the kernel finds the file.txt file. It has inode corresponding to it(i.e, .txt file)

        ![](../images/Inode.png)
    - The information about the file get stored inside the inode object. Inode object of type **struct inode.** 

    - File information contains the information about the file, whereas file data contains the content of the file.

    - Hard disk is divided into Blocks and Sectors.
        
        - Collection of sectors are called blocks.
    - File data get stored in the sectors which is a part of a blocks and the file information is stored in the inode object, and it is also stored in the sectors of Hard disk.

    **Q) Kernel uses which object to represent a file?**

    - Kernel uses inode object too represent the file

    **Q) From user space can we access the file information present in inode object?**

    - Yes, There are 2 types:
        
        1. Using commands from terminal applications

            - **ls -l** Shows the information about each and every file, and the entire information is comming from inode object. 
        
            **Note:**
            - Each file has corresponding inode present in the hard disk . Each inode object will have unique ID and it can be observed using **ls -il**
        2. By using Basic I/O calls form applications
        
            - stat() and fstat()

    **Q) Which system call is used to access the file information? (or) ls command internally uses which system call to access the file information?**
    
    - stat() and fstat() 

    **Q) What happens after kernel finds its inode object?**
    
    1. Inode object corresponding to file get copied from hard disk to the kernel space of RAM.
    2. Kernel creates a new object called file object the kernel content of file objects are:

        ![](../images/File%20Object.png)

        - It also contains the base address of inode kernel uses file object to represent the open files.
        - Whenever the file is created the inode correspondig to file is created but kernel object i.e, file object is created only when we open the file.

    **Q) Difference between primary data and other members of file object?**

    - All the other members are filled/ or used/ or accessed by the kernel but the primary data is not used by the kernel it is used by the drivers.
    - Modes are present in file object and the permissions get stored in struct inode.

    3. The file object base address get stored in fd tavle.
        
        - The first three entries off fd table is stdin, stdout, and stderr and it is inherited from parent process. The base address starts from 4th position indexed 3 onwards in fd table. 

            ![](../images/file%20object%202.png)

        **Q) When fd Table is created?**
        
        - Whenever the process is executed, PCB get created in kernel space of RAM and fd table is the part of the PCB.
        - fd table has nothing to do with open system call.

        **Q) What is the size of fd table?**
        
        - o to 1024

        **Q) When file object is created?**

        - When open() system call is invoked, file object is created.

    - Finally open system call returns index to the table where new entry is created. The indec to the fd table is called fd or file discriptor.

    **Q) What does open() returns?**

    - It returns the value called fd or file discriptor. fd is the index to the fd table which stores the new entries.

    **Q) When file is open() first time in your program what is returns?**

    - 3  (as 0,1,2 is already inherited from parent process/or shell program in fd table).

- **Once we get fd we can do read and write operations.**

## Write() System Call

- File  name is accepted as an arguments by open system call. The remaining basic i/o calls uses "fd" as an argument

```
Syntax: 
        Char buf[1024];
        write(fd,buf,strlen(buf));
```
- From the base address how many address we want to occupy depends upon the third argument i.e, **strlen(buf);**

- **write() ------>  sys_write()**

    - Uses fd as index to the fd table and it will access file object and inode object.
    - It uses file object and inode object while writtingg data into file present in hard disk.
    - The ASCII value of each character is written to file.
    - On error system call return "-1"
    - write returns number of bytes copied/written from user space application to the file present in hard disk.

    **O_TRUNC** &rarr; It will completely erase the whole data.
    **close(fd)** &rarr; Clears the entry in fd table and deallocates the memory.
    **O_CREAT** &rarr; If file does not exist, create a file. 

    ### Read Operation
    ---
    - While reading we assume that file is already existing if it doesnot exist then it will return -1.

    ```
    Synatax:
            char buf[20];
            ret=read(fd,buf,20);
            buf[ret]='\0';
    ```
    - The read() invokes sys_read(3,0xe000,20); It is used first argument fd as index to fd table the file objects and inode objects.

        **Q) Why read need access to file object?**

        1. For checking modes i.e, WRONLY ,RDONLY, or RDWR if there is RDONLY. it will throw error.
        2. For checking the cursor position, which maintains the locations from where we are going to read or write data.

  - When we use basic i/o calls cursor position can not be seen graphically insted cursor position is seen as a value in a file object.
  - Initially the cursor position is at '0' . The position changes while reading and writtig the data.
  - sys_read() will use the file object and inode object for accessing the file present in the hard disk.
  - It reads the data from the file and copy to the address passed as 2nd arguments i.e, buf. In this operation data immediately get copied to the user space array i.e, buf.
  - sys_read at the end returns a value i.e, number of bytes copied from file present in hard disk to user space application i.e, file(Hard disk) &rarr; array(user space application).
  - When the read call completes successfully data is present in the array buf, and to avoid printing garbage value '\0' or null character is added at the end.
   
    ```
    char buf[20];
    ret=read(fd,buf,20);
    buf[ret]='\0';
    printf("\n%s",buf);
    ```
    

   - Such strings does not have any null character thus we add null character at the end.

   ![](../images/Read%20Operation.png)

   ```
   int fd;
   fd=open("file.txt",O_RDONLY);
            {Sends requests to the driver to search whether file file.txt is present in the hard disk or not}
   ```
   - For creating file using open system call. open("file.txt",O_RDWR | O_CREAT, 0640);

   ```
   $ cp file1.txt file2.txt
   ```
        
    1. Creates file2.txt
    2. Copy the contents of file1.txt to file2.txt

    **Requirement:** file.txt should be present with/without same data. 
- Without reading or writting onto file we can change the cursor position by using **lseek()**

**Q) Can I open same file from multiple process. Explain the memory segment in kernel space?**

![](../images/Same%20inode%20for%20multiple%20process.png)

- While opening multiple process, we will have multiple PCB, multiple file objects, but all the file object will point to the same inode object of file.

**Q) Kernel uses which object to represent file?**
 
 &rarr; Inode Object

**Q) Kernel uses which object to represent open file?**

&rarr; file Object.

**Q) Is there any limit on no.of files that can be opened from the program?**

&rarr; Depends upon the size of fd table i.e, depends how many etries we can enable fd table.

### Standard I/O Calls Vs Basic I/O Calls
---
- Standard i/o calls are also called as buffered i/o calls which includes:

    ![](../images/Standard%20io%20calls%20vs%20Basic%20io%20calls.png)


- Other than Basic i/o calls and Standard i/o calls. memory mapping call can be used to access file
- There are two ways to access files using memoey mapping call 
    1. File mapping technique.
    2. Anoymous file mapping technique.

**Q) How do you implement yout own copy commands?**

1. By using basic i/o calls
2. By using standard i/o calls or buffered i/o calls
3. Memory mapping technique(i.e, mmap())


Example: Copy information one file to another file.

```
char buf[1024];
int sfd,dfd;
sfd=open("file1.txt",O_RDONLY);
dfd=open("filw2.txt",O_WRONLY | O_CREAT, 0640);
while((x=read(sfd,buf,1024))>0){
    write(dfd,buf,strlen(buf));
}
```
- Once you reach end of the file read returns 0. The above condition becomes false and loop get terminated.

#### Passing Filename as CLA 
---

- For accessing CLA we need to modify the main program of the c- program

    ```
    main(int argc,char *argv[]){
        - The base address of the CLS is passed to the 2nd arguments.

    sfd=open(argv[1],O_RDONLY);
    dfd=open(argv[2],O_WRONLY | O_CREAT, 0640);
    }

    Program name it self is passed as 1st CLA
    ```
    **Note:**

    - Size of the file is present in the inode objece of file. Inode is a object of type struct inode for inode object number use command **ls -il**.
    - Once the file is opened, the inode object of a corresponding file get copied from hard disk to kernel of RAM.

**Q) How do user space application get accessed to the cotent of inode object present in kernel space?**

- By using Basic I/O Calls  stat() and fstat().

**Q) How do you get access to kernel object/ kernel data structure/ or information present in kernel space?**

- There are three various ways: 

    1. Multiple system calls
    2. Shell command
    3. Proc virtual file system entry.

    ![](../images/inode%20and%20file%20object%20access.png) 

### Use of stat() and fstat():
- Let us assume file.txt is already present in the hard disk.

    ![](../images/fstat()%20use.png)

    ```
    main(){
        int fd,struct stat buf;
        fd=open("file.txt",O_RDWR);
        fstat(fd,&buf);
        printf("%d",buf.st_size);
    }
    ```
    - Initially the buf was empty, once the fstat() call invoke successsfully the variable memory buf present in user space get the content of inode object present in the kernel space.

## Standard File Discriptor

![](../images/File%20Descriptor.png)

- fd table maintains the information about the file that has been opened.
- fd is passed as an argument in all other basic i/o calls except "open call".

**Note:**

- fd '0' when passed as an argument in basic i/o call is used for taking input from keyboard.
- fd '1' and '2' when passed as an argument in basic i/o call is used for printing some information taken from keyboard and error too.

**Q) How to print some string on screen?**

1. printf("Hello world"");
    
    puts("Hello World");
2. System calls
    
    - Basic i/o calls   [write()]
        ```
        main(){
            char buf[20];
            scanf("%s",buf);
            write(1,buf,strlen(buf));
        }
        ```
        write(1,buf,strlen(buf));

        - buf &rarr; address from where we need to import data.
        - 1 &rarr; stdout (fd value).
        
        write(1,"Hello World",strlen("Hello World"));

        - "Hello world" &rarr; Here the base address of the string is present in rodata segment
        - Display Hello world to terminal application, console and screen.
    - printf is the standard library call which internally uses write system call with fd of '1'.

    **Q) How do you prove these strings are stored present in rodata segment?**
    ```
      objdump -s ./a.out  and see the rodata segment. 
    ```

    **Q) Take input from keyboard, store in character array and then display the result?**
        
    ```
    main(){
        char buf[20];
        read(0,buf,20);
        write(1,buf,strlen(buf));
    }
    ```
    - Once write executes successfully it display info onto kernel screen.

    **Note:**

    - Sometimes read behaves as blocking call and sometimes normal call
- When read is used as standard fd i.e, stdin(fd==0) behaves as blocking call. When read is used in fd behaves as normal call. Read accessing special and device files acts as blocking call.
- In case of normal fd, once the cursor position reaches the end of the file and still we are trying to read it, then it will return 0. This, inthis case read is not a blocking call but in above mentioned program read acts as blocking call.

**Need of printf:**

- To display the result.
- Mainly to understand the flow of our program execution.

## dup2() system call

```
dup2() system call is used for duplicating the file discriptor corresponding entries.
```

**How multiple process compile together?**

    
For compilation process:

    1. gcc sample1.c sample2.c
    2. #include "sample.c"
    3. Make file and make utility.

- While compiling the multiple files functions named should never be repeated and they should have one main funcntion. i.e, gcc sample1.c sample2.c

- Larger program will display alots of printfs on screen (i.e, Terminal applications) which increases code size application.
- Once we fit the application (i.e, { } inside it), the entire output is completely lost.

- Inorder to overcome the above problem, by using **redirection** operator:  i.e,  **./a.out > file.txt**

- In this no outputis displayed on the output terminal but the entire output is copied to separate file (i.e, file.txt).

- There are another way to redirect the output to separate file which can be done from c- program by using **dup2() system call**.

    ```
    mainn(){
        int sfd=open("file.txt",O_RDWR);
        if(fd<0){
            printf("Error in open.\n");
            exit(1);
        }
        printf("Statement 1\n);
        dup2(fd,1);
        printf("Mon\n");
        printf("Yadav\n");
        printf("Mousam\n");
    }
    ```
    - dup2() call is going to duplicate the file discriptor

        ![](../images/dup2().png)
    - The 2nd argument of dup2 corresponing to the fd inherited from the init process i.e, stdout, dup2 closes that fd, thus corresponding file object and inode object are destroyed
    - The fd which is passed as first argument of dup2 is copied to the location mentioned by the 2nd argument in fd table.
    - Now fd table 1 is not pointing to the file object and inode object that beings to stdout. It will contain the file object and inode object belonging to file.txt.

    **Advantage of dup2() system call:**
    ```
    No ouput is displayed on terminal screen, the entire output copied and saved to a separate file.
    ```

### Permissions

- Permission are need only during creation of file.
- Permission is represented in octal form i.e, 3bit
- To understand the permissions 
    1) User name/ user id
    2) Group name/group id
    3) Others
- During ubuntu installation, the installation package request a user name and password, from system point of you user name/ unique logic name has corresponding unique numeric id or user if(UID).
- For creating new user account. There is GUL application i.e, **"User account"**.
- A newly created account has:

    - Separate work space for user in home directory (./home) i.e, separate directories, desktop, documents, music and  many more.
- When anu user is created from system point of view UID is created.
- User are created:

    1. During OS Installation
    2. Using GUI application "user account".
    3. CLI (Command Line Interface) "Useradd".
- To understand the permissions for owners and others.we need to understand the **" Users and Groups"**

    **Q) Where do you store ownership information of file?**

    - In inode object, which is present inside the PCB.
- Form user space thw ownership  information of a file can be fetched bu using command.
    ```
    ls -l --> Command
    fstat() --> System call
    ```
- Whatever  information is displayed during ls -l is fetched from the files corresponding to inode object.

**Q) How do you display username corresponding id?**

- **"ls -ln"**

**Q) How to see multiple user information in your system?**
 
- **" cat /etc/passwd "**
- Each line is going to represent single user. Some users name are created by systems where as some are created by user itself.

    - guest -->system cretaed
    - Balu --> User created

**Q) How to see multiple group information in your system?**

- **" cat /etc/group "**
    - guest -GhE51r :x :141:

**Q) Hoe do you change ownership of file?**

1. **System calls**

    - chmod() , fchmod()

    &rarr; chmod("file.txt",UID(2000),GID(4000));

    &rarr; fchmod(fd,2000,4000);
2. **Command(or shell command)**

    - chmod 2000:5000 file.txt

![](../images/UID%20GID.png)

- When you are accessing the file owner checks for corresponding UID and GID

    1. The locked in UID is compared with UID present in file inode object. If it matches then ownership permission is applied.
    2. If locked in UID does not matches, then locked in GID in file inode object is checked. If it matches then group permission is applied.
    3. If UID and GID doesnot matches then others permission is displayed.

**If we do not have match UID and GID but still wants to access the file. It can be done by changing the UID and GID or changing permissions, Otherwise it will throw an error. UID and GID can be changed by using command chmod() and fchmod()**