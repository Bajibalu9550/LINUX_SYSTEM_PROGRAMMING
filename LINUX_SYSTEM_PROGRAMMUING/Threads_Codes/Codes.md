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

