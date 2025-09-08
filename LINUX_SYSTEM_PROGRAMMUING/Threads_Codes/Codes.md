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


