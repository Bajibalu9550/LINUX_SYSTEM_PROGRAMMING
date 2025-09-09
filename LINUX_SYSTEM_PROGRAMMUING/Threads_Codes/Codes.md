# 1. Multithreading in C using `pthread`

This example demonstrates how to create and run a thread in C using the POSIX threads (`pthread`) library.  
The program prints `"Hello World"` from a separate thread.

---

## Code

```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>

void *threadfun(void *arg) {
    printf("%s\n", (char*)arg);
    return NULL;
}

int main() {
    pthread_t ti;
    pthread_create(&ti, NULL, threadfun, "Hello World");
    pthread_join(ti, NULL);

    return 0;
}

OUTPUT:
Hello World
```

# 2. Multi-threading Program in C
## Program Code

```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <stdlib.h>

void *threadfun(void *arg);
void *threadfun(void *arg) {
    printf("Hello this is thread: %d\n", *(int *)arg);
    return NULL;
}

int main() {
    int n = 5;
    pthread_t ti[n];
    int thread_args[n];

    for (int i = 0; i < n; i++) {
        thread_args[i] = i + 1;
        if (pthread_create(&ti[i], NULL, threadfun, &thread_args[i]) != 0) {
            perror("Failed to load\n");
            return 1;
        }
    }

    for (int i = 0; i < n; i++) {
        pthread_join(ti[i], NULL);
    }

    printf("All threads finishing execution.\n");
    return 0;
}
```

# 4. Factorial Using Threads in C

## Program Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *factorial(void *c) {
    int fact = 1;
    int n = *(int *)c;

    while (n != 0) {
        fact = fact * n;
        n--;
    }

    return fact;
}

int main() {
    pthread_t ti;
    int n;

    printf("Enter Number: ");
    scanf("%d", &n);

    if (pthread_create(&ti, NULL, factorial, &n) != 0) {
        perror("Failed to create.\n");
        exit(1);
    }

    int fact;
    pthread_join(ti, &fact);

    printf("Factorial of %d is: %d\n", n, (int)fact);
}
```
# 3. Thread ID Printing Program in C

## Program Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *threadfun(void *n) {
    printf("Thread%d Id: %lu\n", *(int *)n, (unsigned long)pthread_self());
    return NULL;
}

int main() {
    pthread_t t1, t2;
    int id1 = 1, id2 = 2;

    if (pthread_create(&t1, NULL, threadfun, &id1) != 0 || 
        pthread_create(&t2, NULL, threadfun, &id2) != 0) {
        perror("Failed to create.\n");
        exit(1);
    }

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
}
```

# 6. Sum of Two Numbers Using Threads in C

## Program Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

typedef struct {
    int a;
    int b;
    int sum;
} Node;

void *add(void *arg) {
    Node *n = (Node *)arg;
    n->sum = n->a + n->b;
    return NULL;
}

int main() {
    pthread_t ti;
    Node *node = malloc(sizeof(Node));

    printf("Enter Numbers: ");
    scanf("%d %d", &node->a, &node->b);

    if (pthread_create(&ti, NULL, add, node) != 0) {
        perror("Failed to create.\n");
        exit(1);
    }

    pthread_join(ti, NULL);

    printf("Sum of two numbers: %d\n", node->sum);

    free(node);
    return 0;
}
```
# 7. Multithreaded Program to Calculate Square of a Number in C

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

void *square(void *arg) {
    int num = *(int *)arg;
    int *result = malloc(sizeof(int));
    *result = num * num;
    return result;
}

int main() {
    int *n = malloc(sizeof(int));
    if (n == NULL) {
        perror("malloc failed");
        exit(1);
    }

    printf("Enter a number: ");
    scanf("%d", n);

    pthread_t t;
    if (pthread_create(&t, NULL, square, n) != 0) {
        perror("failed to create thread");
        free(n);
        exit(1);
    }

    int *sq;
    pthread_join(t, (void **)&sq);

    printf("Square of num: %d\n", *sq);

    free(n);
    free(sq);
    return 0;
}
```
# 9. Multithreaded Program to Check Prime Number in C

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <math.h>

void *prime(void *arg) {
    int n = *(int *)arg;

    if (n <= 1) {
        printf("Not prime.\n");
        return NULL;
    }

    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i == 0) {
            printf("Not prime.\n");
            return NULL;
        }
    }

    printf("Prime.\n");
    return NULL;
}

int main() { 
    int *n = malloc(sizeof(int));
    if (n == NULL) {
        perror("malloc failed");
        exit(1);
    }

    printf("Enter number: ");
    scanf("%d", n);

    pthread_t t;
    if (pthread_create(&t, NULL, prime, n) != 0) {
        perror("Error create()");
        free(n);
        exit(1);
    }

    pthread_join(t, NULL);
    free(n);

    return 0;
}

```
# 10. Multithreaded Program to Check Palindrome String in C

## Source Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <string.h>

void *palindrome(void *arg) {
    char *str = (char *)arg;
    int j = strlen(str) - 1;
    int i = 0;

    while (i < j) {
        if (str[i] != str[j]) {
            printf("Not Palindrome.\n");
            return NULL;
        }
        i++;
        j--;
    }

    printf("Palindrome\n");
    return NULL;
}

int main() {
    char *str = malloc(100 * sizeof(char));
    if (str == NULL) {
        perror("Failed.\n");
        exit(1);
    }

    printf("Enter string: ");
    fgets(str, 100, stdin);
    str[strlen(str) - 1] = '\0';

    pthread_t t;
    if (pthread_create(&t, NULL, palindrome, str) != 0) {
        perror("Failed to create thread.\n");
        free(str);
        exit(1);
    }

    pthread_join(t, NULL);
    free(str);

    return 0;
}
```
# 11. Multithreaded Program with Synchronization in C

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <stdlib.h>

pthread_mutex_t lock;

void *synchro(void *arg) {
    pthread_mutex_lock(&lock);
    char *str = (char *)arg;
    printf("%s\n", str);
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t1, t2;

    if (pthread_mutex_init(&lock, NULL) != 0) {
        perror("Mutex init failed");
        exit(1);
    }

    if (pthread_create(&t1, NULL, synchro, "Hello World") != 0) {
        perror("Failed t1\n");
        exit(1);
    }

    if (pthread_create(&t2, NULL, synchro, "Hello World") != 0) {
        perror("Failed t2\n");
        exit(1);
    }

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);

    return 0;
}
```
# 12. Mutex Synchronization Example in C (POSIX Threads)

This program demonstrates the use of **pthread mutexes** to synchronize access among threads.  
The mutex ensures that only one thread prints its **Thread ID** at a time, preventing output overlap.

---

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <stdlib.h>

pthread_mutex_t lock;

void *synchro(void *arg) {
    pthread_mutex_lock(&lock);
    printf("Thread ID: %lu\n", (unsigned long)pthread_self());
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t1, t2;

    if (pthread_mutex_init(&lock, NULL) != 0) {
        perror("Mutex init failed");
        exit(1);
    }

    if (pthread_create(&t1, NULL, synchro, NULL) != 0) {
        perror("Failed t1");
        exit(1);
    }

    if (pthread_create(&t2, NULL, synchro, NULL) != 0) {
        perror("Failed t2");
        exit(1);
    }

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);

    return 0;
}
```
# 14. Threaded Addition with Mutex in C

This program demonstrates using **POSIX threads (pthreads)** with a **mutex** for synchronization.  
It creates a thread that adds two integers and returns the result to the main thread.

---

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t lock;

typedef struct {
    int a;
    int b;
} Node;

void *add(void *arg) {
    pthread_mutex_lock(&lock);
    Node *n = (Node *)arg;
    int *result = malloc(sizeof(int));
    *result = n->a + n->b;
    pthread_mutex_unlock(&lock);
    return result;
}

int main() {
    Node node;
    printf("Enter numbers: ");
    scanf("%d %d", &node.a, &node.b);

    pthread_t t;

    if (pthread_mutex_init(&lock, NULL) != 0) {
        perror("Mutex init failed");
        exit(1);
    }

    if (pthread_create(&t, NULL, add, &node) != 0) {
        perror("Thread creation failed");
        exit(1);
    }

    int *ptr;
    pthread_join(t, (void **)&ptr);

    printf("Sum of numbers: %d\n", *ptr);

    free(ptr);
    pthread_mutex_destroy(&lock);

    return 0;
}
```
# 15. Threaded Increment and Decrement with Mutex in C

This program demonstrates how to use **POSIX threads** and a **mutex** to safely increment and decrement a shared variable.

---

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t lock;
int shar_val = 0;

void *increment(void *arg){
    for(int i = 0; i < 5; i++){
        pthread_mutex_lock(&lock);
        shar_val++;
        printf("Increment thread: shar_val = %d\n", shar_val);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

void *decrement(void *arg){
    for(int i = 0; i < 5; i++){
        pthread_mutex_lock(&lock);
        shar_val--;
        printf("Decrement thread: shar_val = %d\n", shar_val);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main(){
    pthread_t t1, t2;

    if(pthread_mutex_init(&lock, NULL) != 0){
        perror("Mutex init failed");
        exit(1);
    }

    if(pthread_create(&t1, NULL, increment, NULL) != 0){
        perror("Thread 1 creation failed");
        exit(1);
    }

    if(pthread_create(&t2, NULL, decrement, NULL) != 0){
        perror("Thread 2 creation failed");
        exit(1);
    }

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Final shared_val = %d\n", shar_val);

    pthread_mutex_destroy(&lock);

    return 0;
}
```
# 16. Develop a C program to create a thread that reads input from the user and synchronizes access to shared resources?

This program demonstrates creating a **POSIX thread** that reads input from the user and uses a **mutex** to synchronize access to a shared resource.

---

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t lock;
char str[1000];

void *string(void *arg){
    pthread_mutex_lock(&lock);
    printf("Enter text: ");
    fgets(str, sizeof(str), stdin);
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main(){
    pthread_t t1;

    if(pthread_mutex_init(&lock, NULL) != 0){
        perror("Mutex init failed");
        exit(1);
    }

    if(pthread_create(&t1, NULL, string, NULL) != 0){
        perror("Thread creation failed");
        exit(1);
    }

    pthread_join(t1, NULL);

    pthread_mutex_lock(&lock);
    printf("Entered string: %s\n", str);
    pthread_mutex_unlock(&lock);

    pthread_mutex_destroy(&lock);

    return 0;
}
```
# 17.Implement a C program to create a thread that prints prime numbers up to a given limit with mutex locks?
---
Write a C program to create a thread that prints all prime numbers up to a given limit.
The program should use mutex locks to synchronize access to shared resources (here, printf).
## Source Code

```c

#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <math.h>

pthread_mutex_t lock;

int isprime(int n) {
    if (n < 2) return 0;
    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i == 0) {
            return 0;
        }
    }
    return 1;
}

void *print_primes(void *arg) {
    int n = *(int *)arg;
    for (int i = 2; i <= n; i++) {
        if (isprime(i)) {
            pthread_mutex_lock(&lock);
            printf("%d ", i);
            pthread_mutex_unlock(&lock);
        }
    }
    printf("\n");
    return NULL;
}

int main() {
    pthread_t t1;
    int n;

    printf("Enter limit: ");
    scanf("%d", &n);

    if (n < 2) {
        printf("No primes below 2\n");
        return 0;
    }

    if (pthread_mutex_init(&lock, NULL) != 0) {
        perror("Mutex init failed\n");
        exit(1);
    }

    if (pthread_create(&t1, NULL, print_primes, &n) != 0) {
        perror("Thread creation failed\n");
        exit(1);
    }

    pthread_join(t1, NULL);
    pthread_mutex_destroy(&lock);

    return 0;
}
```
# 19.Write a C program to create a thread that checks if a given year is a leap year using dynamic programming with mutex locks? 

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t lock;

void *leapyear(void *arg) {
    int n = *(int *)arg;
    pthread_mutex_lock(&lock);

    if ((n % 100 != 0 && n % 4 == 0) || (n % 400 == 0)) {
        printf("Leap year\n");
    } else {
        printf("Non leap year\n");
    }

    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t;
    int *year = malloc(sizeof(int));

    printf("Enter Year: ");
    scanf("%d", year);

    if (pthread_mutex_init(&lock, NULL) != 0) {
        perror("Failed init\n");
        exit(1);
    }

    if (pthread_create(&t, NULL, leapyear, year) != 0) {
        perror("Failed thread\n");
        exit(1);
    }

    pthread_join(t, NULL);
    pthread_mutex_destroy(&lock);
    free(year);

    return 0;
}
```
# 20.Write a C program to create a thread that checks if a given string is a palindrome using dynamic programming with mutex locks? 

## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <string.h>

pthread_mutex_t lock;

void *check_palindrome(void *arg) {
    char *ptr = (char *)arg;
    int i = 0, j = strlen(ptr) - 1;
    int p = 1;

    while (i < j) {
        pthread_mutex_lock(&lock);
        if (ptr[i] != ptr[j]) {
            p = 0;
            pthread_mutex_unlock(&lock);
            break;
        }
        i++;
        j--;
        pthread_mutex_unlock(&lock);
    }

    if (p) {
        printf("Palindrome.\n");
    } else {
        printf("Not Palindrome.\n");
    }

    return NULL;
}

int main() {
    pthread_t t;
    char *str = malloc(100 * sizeof(char));

    if (str == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }

    printf("Enter string: ");
    fgets(str, 100, stdin);
    str[strlen(str) - 1] = '\0';  

    if (pthread_mutex_init(&lock, NULL) != 0) {
        perror("Failed init\n");
        exit(1);
    }

    if (pthread_create(&t, NULL, check_palindrome, str) != 0) {
        perror("Failed thread\n");
        exit(1);
    }

    pthread_join(t, NULL);
    pthread_mutex_destroy(&lock);
    free(str);

    return 0;
}
```

# 21.Implement a C program to create a thread that performs selection sort on an array of integers? 
