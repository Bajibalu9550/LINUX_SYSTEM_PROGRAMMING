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
# 3. Create a C program to ignore the SIGCHLD signal temporarily.
```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

int main() {
    pid_t pid;

    printf("Parent process PID: %d\n", getpid());

    // Step 1: Ignore SIGCHLD
    printf("Temporarily ignoring SIGCHLD...\n");
    signal(SIGCHLD, SIG_IGN);

    pid = fork();
    if (pid < 0) {
        perror("fork failed");
        exit(EXIT_FAILURE);
    } else if (pid == 0) {
        // Child process
        printf("Child process (PID: %d) started.\n", getpid());
        sleep(2);
        printf("Child process (PID: %d) exiting.\n", getpid());
        exit(0);
    } else {
        // Parent process
        printf("Parent waiting while ignoring SIGCHLD...\n");
        sleep(4);

        // Step 2: Restore default signal handling
        printf("\nRestoring default SIGCHLD behavior.\n");
        signal(SIGCHLD, SIG_DFL);

        // Create another child after restoring default behavior
        pid = fork();
        if (pid == 0) {
            printf("New child process (PID: %d) started.\n", getpid());
            sleep(2);
            printf("New child process (PID: %d) exiting.\n", getpid());
            exit(0);
        } else {
            // Wait for child termination normally
            wait(NULL);
            printf("Parent reaped the new child process.\n");
        }
    }

    printf("Parent process exiting.\n");
    return 0;
}

```
# 5. Implement a C program to handle the SIGALRM signal using sigaction().
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void alarm_handler(int signo){
        printf("Caught sig{SIGALRM): %d\n",signo);
        printf("Alarm signal handled successfully.\n");
        exit(1);
}
int main(){

        struct sigaction st;

        st.sa_handler=alarm_handler;
        st.sa_flags=0;
        sigemptyset(&st.sa_mask);

        if(sigaction(SIGALRM,&st,NULL)==-1){
                perror("sigaction");
                exit(EXIT_FAILURE);
        }

        printf("Setting alarm for 5 seconds.\n");
        alarm(5);

        printf("Waiting for alarm signal.\n");
        while(1){
                //pause();
                printf("Running...\n");
                sleep(1);
        }
}


```
# 6. Write a C program to install a custom signal handler for SIGTERM?
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>

void handler(int signo){
        printf("Caught sig: %d\n",signo);

        sleep(1);
        exit(0);

}
int main(){
        struct sigaction act;

        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);

        if(sigaction(SIGTERM,&act,NULL)==-1){
                perror("sigaction");
                exit(EXIT_FAILURE);
        }
        printf("Waiting for signal(SIGTERM): ");

        while(1){
                printf("Running..\n");
                sleep(1);
        }
}

```
# 7. Implement a program to handle the SIGSEGV signal (segmentation fault).
````c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

// Signal handler for SIGSEGV
void segfault_handler(int signo) {
    printf("\nCaught signal %d (Segmentation Fault)\n", signo);
    printf("Segmentation fault detected! Exiting gracefully.\n");
    exit(EXIT_FAILURE);
}

int main() {
    // Register the signal handler
    if (signal(SIGSEGV, segfault_handler) == SIG_ERR) {
        perror("signal");
        exit(EXIT_FAILURE);
    }

    printf("This program will cause a segmentation fault.\n");

    // Deliberately cause a segmentation fault
    int *ptr = NULL;
    printf("Value at ptr: %d\n", *ptr);  // Dereferencing NULL causes SIGSEGV

    printf("This line will not be executed.\n");

    return 0;
}

````
# 8. Create a program to handle the SIGILL signal (illegal instruction).
```c

#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>

void handler(int signo){
        printf("Caught the signal: %d (SIGILL).\n",signo);
        exit(1);
}
int main(){
        signal(SIGILL,handler);

        printf("Perform illegal instruction.\n");

        asm("ud2");
        printf("This line will never execute.\n");
}

```
# 9. Write a program to handle the SIGABRT signal (abort).
```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigabrt(int signo) {
    printf("Caught signal: %d (SIGABRT)\n", signo);
    printf("Program aborted intentionally using abort().\n");
    exit(1);
}

int main() {
    signal(SIGABRT, handle_sigabrt);

    printf("Before abort() call.\n");

    abort();

    printf("This line will not be executed.\n");
    return 0;
}


```
# 10. Implement a C program to handle the SIGQUIT signal.
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int signo){
        printf("Caught a signal: %d (SIGQUIT).\n",signo);
        printf("Exiting forcefully.\n");
        exit(1);
}
int main(){
        signal(SIGQUIT,handler);
        printf("Waiting for the signal (SIGQUIT).\n");;
        while(1){
                printf("Running...\n");
                sleep(1);
        }
}

```
# 11. Write a program to handle the SIGTERM signal (termination request).

```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int signo){
        printf("Caught a signal: %d (SIGTERM).\n",signo);
        printf("Exiting forcefully.\n");
        exit(1);
}
int main(){
        printf("Process id: %d\n",getpid());
        signal(SIGTERM,handler);
        printf("Waiting for the signal (SIGTERM).\n");;
        while(1){
                printf("Running...\n");
                sleep(1);
        }
}



```
