# 43. Implement a program that uses Named pipes for communication between two processes.


```c
===============================SERVER========================
#include<errno.h>
#include<string.h>
#include<fcntl.h>
#include<sys/stat.h>
int main(){
        int ret;
        char buf[1024];
        int srcfd=open("srvfifo",O_RDONLY);
        if(srcfd<0){
                if(mkfifo("srvfifo",0640)<0){
                        printf("mkfifo error: %s\n",strerror(errno));
                        return 1;
                }
        }
        else {
                printf("Parent waiting.\n");
                ret=read(srcfd,buf,sizeof(buf));
                buf[ret]='\0';
                printf("Parent recevied: %s\n",buf);

                close(srcfd);
        }
}

====================================CLIENT==========================

#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
#include<errno.h>
#include<string.h>
int main(){
        int srcfd=open("srvfifo",O_WRONLY);
        char str[1024];
        if(srcfd<0){
                printf("open faile: %s\n",strerror(errno));
                return 1;
        }
        printf("Enter string: ");

        fgets(str,sizeof(str),stdin);
        str[strlen(str)-1]='\0';
        write(srcfd,str,strlen(str));
        close(srcfd);
}

```
