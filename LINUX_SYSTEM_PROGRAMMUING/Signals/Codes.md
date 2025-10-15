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
### Sender

```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>

int main(){
        int pid,sig;
        printf("Enter process id: ");
        scanf("%d",&pid);

        printf("Enter signal: (10 for SIGUSR1 or 12 for SIFUSR2): ");
        scanf("%d",&sig);

        if(kill(pid,sig)==-1){
                perror("kill");
                exit(EXIT_FAILURE);
        }

        printf("signal sent successfully.\n");
}

```
### Receiver
```c

#include<unistd.h>

void handler(int signo){
        printf("Recieved signal: %d\n",signo);
        if(signo == 10){
                printf("Custom message: SIGUSR1 Received.\n");
        }
        else if(signo == 12){
                printf("Custom message: SIGUSR2 Received.\n");
        }
}
int main(){
        signal(SIGUSR1,handler);
        signal(SIGUSR2,handler);

        printf("Receiver waiting for signal.\n");

        while(1){
        //      printf("Sleeeping....\n");
        //      sleep(1);
                pause();
        }
}

```
