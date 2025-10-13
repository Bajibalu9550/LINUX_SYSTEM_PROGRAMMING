# 1. Write a C program to catch and handle the SIGINT signal\
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<stdlib.h>
void sigHandler(int signo){
        char choice;
        printf("\nCaught signal: %d(SIGINT) You pressed Ctrl+c.\n",signo);
        printf("Really you want to exit (y/n): ");
        scanf("%c",&choice);
        if(choice=='y' || choice=='Y'){
                printf("Exiting program....\n");
                exit(0);
        }
        else {
                printf("Continue programming...\n");
        }
}
int main(){
        signal(SIGINT,sigHandler);

        while(1){
                printf("Running...\n");
                sleep(1);
        }
}

```
# 2. Implement a C program to send a custom signal to another process.
