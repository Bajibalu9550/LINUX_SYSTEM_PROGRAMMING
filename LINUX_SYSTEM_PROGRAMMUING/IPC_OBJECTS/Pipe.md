# 41. working of pipe communication example between parent and child.

```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<errno.h>


int main(){
        int fds[2];
        int n;
        char str[1024];
        char read_msg[1024];

        if(pipe(fds)==-1){
                printf("Error at pipe creation: %s\n",strerror(errno));
                return 1;
        }

        int pid=fork();

        if(pid<0){
                printf("Error at fork: %s\n",strerror(errno));
                return 1;
        }

        else if(pid==0){
                close(fds[1]);
                int z=read(fds[0],read_msg,sizeof(str));
                read_msg[z]='\0';
                printf("Child reads data from parent:  %s\n",read_msg);
                close(fds[0]);

        }
        else {
                close(fds[0]);
                printf("This is parent process: Enter msg for child process:\n");
                fgets(str,sizeof(str),stdin);
                str[strlen(str)-1]='\0';
                write(fds[1],str,strlen(str));
                close(fds[1]);
        }
}


```

# 42. Create a program where two processes communicate synchronously using pipes, Ensure that one process waits for the other to finish before proceeding.

```c

#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<errno.h>
#include<stdlib.h>

int main(){
        int p2c[2];
        int c2p[2];

        int pid;
        char write_data[1024],read_data[1024];

        if(pipe(p2c)==-1 || pipe(c2p)==-1){
                printf("pipe: %s\n ",strerror(errno));
                exit(1);
        }

        pid=fork();
        if(pid<0){
                printf("fork failed: %s\n",strerror(errno));
                exit(1);
        }

        else if(pid==0){
                close(p2c[1]);
                close(c2p[0]);

                int n=read(p2c[0],read_data,sizeof(read_data));

                read_data[n]='\0';
                printf("Child received: %s\n",read_data);

                char reply[1024];
                printf("Child process: ");
                fgets(reply,sizeof(reply),stdin);
                reply[strlen(reply)-1]='\0';
                write(c2p[1],reply,strlen(reply));

                close(p2c[0]);
                close(c2p[1]);
        }
        else {
                close(p2c[0]);
                close(c2p[1]);

                printf("This is parent process: ");
                fgets(write_data,sizeof(write_data),stdin);
                send[strlen(write_data)-1]='\0';
                write(p2c[1],write_data,strlen(write_data));

                char send[1024];
                int n=read(c2p[0],send,sizeof(send));
                send[n]='\0';
                printf("Parent received: %s\n",send);

                close(p2c[0]);
                close(c2p[1]);
        }
}

```
