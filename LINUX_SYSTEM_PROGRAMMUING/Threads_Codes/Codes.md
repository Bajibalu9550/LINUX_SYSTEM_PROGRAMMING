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


## Source Code

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

void *sort(void *arg){
    int *arr = (int *)arg;
    int n = arr[0]; 
    int *a = &arr[1]; 

    for(int i=0; i<n-1; i++){
        for(int j=i+1; j<n; j++){
            if(a[i] > a[j]){
                int temp = a[i];
                a[i] = a[j];
                a[j] = temp;
            }
        }
    }

    printf("Sorted array: ");
    for(int i=0; i<n; i++){
        printf("%d ", a[i]);
    }
    printf("\n");
    return NULL;
}

int main(){
    int n;
    printf("Enter size of array: ");
    scanf("%d", &n);

    int *arr = malloc((n+1) * sizeof(int));
    arr[0] = n; 
    printf("Enter array elements: ");
    for(int i=0; i<n; i++){
        scanf("%d", &arr[i+1]);
    }

    pthread_t t;
    pthread_create(&t, NULL, sort, arr);
    pthread_join(t, NULL);

    free(arr);
    return 0;
}
```
# 22. Area of triangle

## Source Code

```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>

struct triangle{
        float b;
        float h;
};

void *area_tr(void *arg){
        struct triangle *t=(struct triangle*)arg;
        printf("%.2f\n",0.5*(t->b)*(t->h));

        return NULL;
}

int  main(){
        pthread_t t1;
        struct triangle t;
         printf("Enter heighr: ");
         scanf("%f",&t.h);
         printf("Enter base: ");
         scanf("%f",&t.b);

         if(pthread_create(&t1,NULL,area_tr,&t)!=0){
                 perror("thread1.\n");
                 exit(1);
         }

         pthread_join(t1,NULL);
}

```
# 23. C Program to Calculate the Sum of Squares of First 100 Natural Numbers Using Threads

## Source Code

```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#include<unistd.h>
void *sum(void *arg){
        long int *total=malloc(sizeof(long int));
        for(int i=1;i<=100;i++){
                total+=(i*i);
        }
        return total;
}
int main(){
        pthread_t t1;

        if(pthread_create(&t1,NULL,sum,NULL)!=0){
                perror("Failed thread.\n");
                exit(1);
        }
        long int **s;
        pthread_join(t1,(void**)&s);
        printf("Sum: %ld\n",*(long int**)&s);
}
```
# 24. Write a C Program to create a thread that generates a random array of integers?

## Source Code

```c
#include<pthread.h>
#include<unistd.h>
#include<time.h>

#define MAX 1000

int arr[MAX];
int n;

void *random_generate(void *arg){
        int rand=*(int*)arg;
        srand(time(0));
        for(int i=0;i<n;i++){
                arr[i]=random() % rand +1;
        }
        return NULL;
}

int main(){
        pthread_t t1;
        int *rand=malloc(sizeof(int));
        printf("Enter size of array (<=%d): ",MAX);
        scanf("%d",&n);
        if(n<=0 || n>MAX){
                printf("Array size invalid.\n");
                exit(1);
        }
        printf("Enter range of random: ");
        scanf("%d",rand);
        if(pthread_create(&t1,NULL,random_generate,rand)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);

        printf("Random generated array: ");

        for(int i=0;i<n;i++){
                printf("%d ",arr[i]);
        }
        printf("\n");


}
```
# 25. Implement a thread that performs bubble sort

## Source Code

```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#include<unistd.h>

void *bubble_sort(void *arg){
        int *arr=(int *)arg;
        int n=arr[0];
        int *a=&arr[1];
        for(int i=0;i<n-1;i++){
                for(int j=0;j<n-i-1;j++){
                        if(a[j]>a[j+1]){
                                a[j]=(a[j]+a[j+1]) - (a[j+1]=a[j]);
                        }
                }
        }
        return NULL;
}

int main(){
        pthread_t t1;
        //Node node;
        int n;
        printf("Enter size of array: ");
        scanf("%d",&n);
        int *arr=malloc((n+1)*sizeof(int));
        arr[0]=n;
        printf("Enter array elements: ");
        for(int i=0;i<n;i++){
                scanf("%d",&arr[i+1]);
        }

        if(pthread_create(&t1,NULL,bubble_sort,arr)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);

        printf("Sorted array: ");
        for(int i=0;i<n;i++){
                printf("%d ",arr[i+1]);
        }
}
```
# 26. Implement a thread to find GCD of two numbers
## Source Code

```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
typedef struct number{
        int a;
        int b;
        int c;
}Node;

void *gcd(void *arg){
        Node *ptr=(Node *)arg;
        int a=ptr->a;
        int b=ptr->b;
        while(b!=0){
                int temp=b;
                b=a%b;
                a=temp;
        }
        ptr->c=a;
        return NULL;
}
int main(){
        pthread_t t;

        Node node;

        printf("Enter Numbers: ");
        scanf("%d %d",&node.a,&node.b);

        if(pthread_create(&t,NULL,gcd,&node)!=0){
                perror("failed thread.\n");
                exit(1);
        }
         pthread_join(t,NULL);

         printf("GCD of %d and %d : %d\n",node.a,node.b,node.c);
}
```
# 27. Sum of fibonacci series upto limit.

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<unistd.h>
typedef struct fibb{
        int limit;
        long long int sum;
}node;
void *fib(void *arg){
        node *data=(node *)arg;

        int a=0,b=1,next=0,sum=0;
        for(int i=1;i<=data->limit;i++){
                sum+=a;
                next=a+b;
                a=b;
                b=next;
        }
        data->sum=sum;
        return NULL;
}

int main(){
        pthread_t t1;
        node data;
        printf("Enter limit: ");
        scanf("%d",&data.limit);
        if(pthread_create(&t1,NULL,fib,&data)!=0){
                perror("failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        printf("Sun of fibonacci series upto: %lld\n",data.sum);
}

```

# 28. Implement a C program that create thread to calculate sum of even numbers from 1 to 100
## Source Code

```c

#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

void *sum_even(void *arg){
        int *sum=malloc(sizeof(int));
        *sum=0;
        for(int i=2;i<=100;i+=2){
                *sum+=i;
                printf("%d ",i);
        }
        return sum;
}
int main(){
        pthread_t t1;
        int *sum1;
        if(pthread_create(&t1,NULL,sum_even,NULL)!=0){
                perror("failed thread.\n");
                exit(1);
        }

        pthread_join(t1,(void**)&sum1);
        printf("\nSum: %d\n",*sum1);
        free(sum1);

}
```
# 29. Wriite a C Program create a thread that calculates factorial of a given number using iterations.
## Source Code

```c

#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>

void *factorial(void *arg){
        int n=*(int*)arg;
        long int *fact=malloc(sizeof(long int));
        *fact=1;
        for(int i=1;i<=n;i++){
                *fact*=i;
        }
        return fact;
}
int main(){
        pthread_t t1;
        int n;
        printf("Enter Number: ");
        scanf("%d",&n);
        long int *ptr;
        if(pthread_create(&t1,NULL,factorial,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,(void**)&ptr);
        printf("Factorial of %d is: %ld\n",n,*ptr);
        free(ptr);
}
```

# 30. Write a C Program create thread check if the given year is leap year or not.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>

void *leapyear(void *arg){
        int year=*(int *)arg;
        if((year%4==0 && year %100!=0) || (year%400==0)){
                printf("%d is leap year.\n",year);
        }
        else {
                printf("%d is Not leap year.\n",year);
        }
        return NULL;
}
int main(){
        pthread_t t1;
        int year;
        printf("Enter Year: ");
        scanf("%d",&year);
        if(pthread_create(&t1,NULL,leapyear,&year)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
}

```
# 32. Average of 1 to 100 numbers.

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<unistd.h>

void *average(void *arg){
        float *av=malloc(sizeof(float));
        *av=0;
        int sum=0;
        for(int i=1;i<=100;i++){
                sum+=i;
        }
        *av=(float)sum/100;
        return av;
}

int main(){
        pthread_t t1;

        if(pthread_create(&t1,NULL,average,NULL)!=0){
                perror("Failed.\n");
                exit(1);
        }
        float *avg;
        pthread_join(t1,(void **)&avg);
        printf("Average of numbers upto 100: %.2f\n",*avg);
        free(avg);
}
```

# 33. Implement a thread that generate random string.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<string.h>

typedef struct str{
        int length;
        char *result;
}Node;


void *randomstring(void *arg){
        Node *data=(Node*)arg;

        char charset[]="abcdefghijklmnopqrstuvwxyz""ABCDEFGHIJKLMNOPQRSTUVWXYZ""1234567890";
        int ch=strlen(charset);
        for(int i=0;i<data->length;i++){
                int len=rand() % ch;
                data->result[i]=charset[len];
        }
        data->result[data->length]='\0';
        pthread_exit(NULL);
}
int main(){
        int len;
        printf("Enter string length: ");
        scanf("%d",&len);

        char *str=malloc(len+1);
        if(str==NULL){
                printf("Memory allocation failed.\n");
                exit(1);
        }
        Node node;
        pthread_t t1;

        node.length=len;
        node.result=str;
        srand(time(NULL));
        if(pthread_create(&t1,NULL,randomstring,&node)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        printf("Random string: %s\n",str);
        free(str);
}

```
# 34. Chack whether the given number is perfect square or not.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<math.h>
void *perfect(void *arg){
        long int n=*(long int*)arg;

        long int i=sqrt(n);
                if(i*i==n){
                        printf("%ld is Perfect square.\n",n);
                        return NULL;
                }

        printf("%ld is not perfect square.\n",n);
        return NULL;
}

int main(){
        long int per;
        printf("Enter number: ");
        scanf("%ld",&per);

        pthread_t t1;

        if(pthread_create(&t1,NULL,perfect,&per)!=0){
                perror("Failed.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
}
```
# 35. Create a threadSum of digits in given number
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>

void *sum_of_digits(void *arg){
        int d=*(int *)arg;
        int sum=0;
        while(d!=0){
                sum+=(d%10);
                d/=10;
        }
        printf("Sum of digits in given number: %d\n",sum);
        return NULL;
}
int  main(){
        pthread_t t1;
        int n;
        printf("Enter Number: ");
        scanf("%d",&n);

        if(pthread_create(&t1,NULL,sum_of_digits,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
}
```
# 36. Write a C program create a thread to find the factorial of number using recursion.

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>

long long int facto(int n){
        if(n==0 || n==1)
                return 1;
        return n*facto(n-1);
}
void *factorial(void *arg){
        int d=*(int *)arg;
        long long int *fac=malloc(sizeof(long long int));
        *fac=0;
        *fac=facto(d);
        return fac;
}
int main(){
        int n;
        printf("Enter Number: ");
        scanf("%d",&n);

        pthread_t t1;

        if(pthread_create(&t1,NULL,factorial,&n)!=0){
                perror("Failed.\n");
                exit(1);
        }
        long long int *fact;
        pthread_join(t1,(void **)&fact);
        printf("Factorial of %d is: %lld\n",n,*fact);
}
```
# 37. Develop a C Program create a thread that find out the maximum element in array. 
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#define MAX 1000
int a[MAX];


void *longest(void *arg){
        int n=*(int*)arg;
        int *max=malloc(sizeof(int));
        *max=a[0];
        for(int i=1;i<n;i++){
                if(*max<a[i])
                        *max=a[i];
        }
        return max;
}

int main(){
        int n;
        printf("Enter array length(<%d): ",MAX);
        scanf("%d",&n);

        printf("Enter array elements: ");
        for(int i=0;i<n;i++){
                scanf("%d",&a[i]);
        }
        pthread_t t1;

        if(pthread_create(&t1,NULL,longest,&n)!=0){
                perror("Failed thread.");
                exit(1);
        }
        int *max1;

        pthread_join(t1,(void **)&max1);

        printf("Largest element in the array: %d\n",*max1);
        free(max1);
}
```
# 38. Write a C program to create a thread and sort array strings.
## Source Code

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<pthread.h>
#define MAX 50
char *arr[50];


void *sort_strings(void *arg){
        int n=*(int *)arg;
        char *temp;
        for(int i=0;i<n-1;i++){
                for(int j=i+1;j<n;j++){
                        if(strcmp(arr[i],arr[j])>0){
                                temp=arr[i];
                                arr[i]=arr[j];
                                arr[j]=temp;
                        }
                }
        }
        return NULL;
}
int main(){
        int n;
        printf("Enter number of strings:(<%d): ",MAX);
        scanf("%d",&n);
        getchar();
        for(int i=0;i<n;i++){
                arr[i]=malloc(100);
                if(arr[i]==NULL){
                        printf("Memory allocation failed.\n");
                        exit(1);
                }
                printf("Enter %d string: ",i+1);
                fgets(arr[i],100,stdin);
                arr[i][strlen(arr[i])-1]='\0';
        }
        pthread_t t1;

        if(pthread_create(&t1,NULL,sort_strings,&n)!=0){
                printf("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        printf("Sorted List: ");
        for(int i=0;i<n;i++){
                printf("%s ",arr[i]);
        }
        for(int i=0;i<n;i++)

            free(a[i]);
        printf("\n");
}
```
# 39. Write a program to create thread that calculate the square root of number.
## Source Code

```c
#include<stdio.h>
#include<math.h>
#include<pthread.h>
#include<stdlib.h>


void *square_root(void *arg){
        int n=*(int *)arg;
        float *f=malloc(sizeof(float));
        *f=sqrt(n);
        return f;
}

int main(){
        int n;
        printf("Enter Number: ");
        scanf("%d",&n);
        pthread_t t1;
        if(pthread_create(&t1,NULL,square_root,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        float *f;

        pthread_join(t1,(void **)&f);
        printf("Square root of number %d is: %.3f\n",n,*f);
        free(f);
}
```
# 40. Write a C program create thread find average of array elements.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#define MAX 100

int a[MAX];

void *average(void *arg){
        int n=*(int *)arg;
        float sum=0;
        for(int i=0;i<n;i++){
                sum+=a[i];
        }
        float *f=malloc(sizeof(float));
        *f=sum/n;
        return f;
}
int main(){
        int n;
        printf("Enter array size:(<%d): ",MAX);
        scanf("%d",&n);
        if(n<0 || n>MAX){
                printf("Enter array size between 1 to %d\n",MAX);
                return 1;
        }


        printf("Enter array elements: ");
        for(int i=0;i<n;i++){
                scanf("%d",&a[i]);
        }

        pthread_t t1;

        if(pthread_create(&t1,NULL,average,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        float *f;
        pthread_join(t1,(void **)&f);

        printf("Average of array: %.2f\n",*f);
        free(f);
}
```
# 41. Write a C Program to create thread and generate random password.

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>
#include<time.h>

void *genarate_password(void *argg){
        int n=*(int *)argg;

        char ch[]="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890!@#$%^&*?";
        int len=strlen(ch);
        char *pass=malloc(n+1);
        if(pass==NULL){
                printf("Memory allocation failed.\n");
                exit(1);
        }
        for(int i=0;i<n;i++){
                pass[i]=ch[rand() % len];
        }
        pass[n]='\0';
        return pass;
}

int main(){
        int length;
        printf("Enter length of password do you want: ");
        scanf("%d",&length);
        pthread_t t1;
        srand(time(NULL));
        if(pthread_create(&t1,NULL,genarate_password,&length)!=0){
                perror("Failed thread.\n");
                exit(1);
        };
        char *ch1;
        pthread_join(t1,(void **)&ch1);
        printf("Genarated password: %s\n",ch1);
        free(ch1);
}
```
# 42. Write a program to create thread given number even or not.
## Source Code

```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
int g=0;

void *even(void *arg){
        int n=*(int *)arg;
        if(n%2==0)
                g=1;
        return NULL;
}
int main(){
        int n;
        printf("Enter Number: ");
        scanf("%d",&n);

        pthread_t t1;

        if(pthread_create(&t1,NULL,even,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }
        pthread_join(t1,NULL);
        if(g){
                printf("Even\n");
        }
        else {
                printf("Odd\n");
        }
}
```
# 43. Write a C Program that creates thread and sum of array elements.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#define MAX 100

int a[MAX];

void *average(void *arg){
        int n=*(int *)arg;
        int *sum=malloc(sizeof(int));
        *sum=0;
        for(int i=0;i<n;i++){
                *sum+=a[i];
        }
        return sum;
}
int main(){
        int n;
        printf("Enter array size:(<%d): ",MAX);
        scanf("%d",&n);
        if(n<0 || n>MAX){
                printf("Enter array size between 1 to %d\n",MAX);
                return 1;
        }


        printf("Enter array elements: ");
        for(int i=0;i<n;i++){
                scanf("%d",&a[i]);
        }

        pthread_t t1;

        if(pthread_create(&t1,NULL,average,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        int *f;
        pthread_join(t1,(void **)&f);

        printf("Sum of array: %d\n",*f);
        free(f);
}
```
# 44. Print Factorial of numbers from 1 to 10.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

int factorial(int n){
        int i=1,fact=1;
        while(i<=n){
                fact*=i;
                i++;
        }
        return fact;
}
void *fact(void *arg){
        for(int i=1;i<=10;i++){
                printf("Factorial of %d is: %d\n",i,factorial(i));
        }
        return NULL;
}
int main(){
        pthread_t t1;
        if(pthread_create(&t1,NULL,fact,NULL)!=0){
                perror("Failed thred.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
}
```
# 45. Calculate the area of rectangle.
## Source Code

```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
typedef struct ar{
        float length;
        float breadth;
        float area;
}Node;

void *rectangle(void *arg){
        Node *ptr=(Node *)arg;

        ptr->area = ptr->length * ptr->breadth;

        return NULL;
}
int main(){

        Node node;
        printf("Enter length and breadth: ");
        scanf("%f %f",&node.length,&node.breadth);

        pthread_t t1;
        if(pthread_create(&t1,NULL,rectangle,&node)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);

        printf("Area of rectangle: %.2f\n",node.area);
}
```
# 46. Create a thread that generate random numbers
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<time.h>

void *randomm(void *arg){
        int n=*(int *)arg;

        for(int i=0;i<n;i++){
                printf("Generated %dst number: %d\n",i+1,rand() %100);
        }
        return NULL;
}
int main(){
        int n;
        pthread_t t1;
        printf("Enter Number: ");
        scanf("%d",&n);

        srand(time(0));

        if(pthread_create(&t1,NULL,randomm,&n)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
}
```
# 47. Write a program to sort an array.

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#define MAX 100

int a[MAX];

void *sort(void *arg){
        int n=*(int *)arg;

        for(int i=0;i<n-1;i++){
                for(int j=0;j<n-i-1;j++){
                        if(a[j]>a[j+1]){
                                a[j]=(a[j] + a[j+1]) - (a[j+1]=a[j]);
                        }
                }
        }
        return NULL;
}
int main(){
        int n;
        printf("Enter array size: ");
        scanf("%d",&n);
        printf("Enter array elements: ");
        for(int i=0;i<n;i++){
                scanf("%d",&a[i]);
        }
        pthread_t t1;
        if(pthread_create(&t1,NULL,sort,&n)!=0){
                perror("Failed to load.\n");
                exit(1);
        }
        pthread_join(t1,NULL);

        printf("Sorted array: ");
        for(int i=0;i<n;i++){
                printf("%d ",a[i]);
        }
        printf("\n");
}
```
# 48. Write a C program to create thread and search for an element.

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

typedef struct node{
        int len;
        int *a;
        int n;
}Node;

void *sort(void *arg){
        Node *ptr=(Node *)arg;
        int *flag=malloc(sizeof(int));
        *flag=0;
        for(int i=0;i<ptr->len;i++){
                if(ptr->a[i]==ptr->n){
                        *flag=1;
                        break;
                }
        }

        return flag;
}
int main(){
        pthread_t t1;
        Node node;
        printf("Enter array size: ");
        scanf("%d",&node.len);
        int *arr=malloc(sizeof(int) *node.len);
        if(arr==NULL){
                printf("Memory allocation failed.\n");
                exit(1);
        }

        node.a=arr;
        printf("array elements: ");
        for(int i=0;i<node.len;i++){
                scanf("%d",&node.a[i]);
        }

        printf("Enter searching element: ");
        scanf("%d",&node.n);
        if(pthread_create(&t1,NULL,sort,&node)!=0){
                perror("Failed to load.\n");
                exit(1);
        }
        int *fl;
        pthread_join(t1,(void **)&fl);
        if(*fl)
                printf("Present.\n");
        else
                printf("Not present.\n");

        free(fl);
        free(arr);
}
```
# 49. Write a program to create that reverse a string.
## Source Code

```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
#include<stdlib.h>

void *reverse(void *arg){
        char *ptr=(char*)arg;
        int j=strlen(ptr)-1;
        int i=0;
        while(i<j){
                ptr[i]=ptr[i]^ptr[j];
                ptr[j]=ptr[i]^ptr[j];
                ptr[i]=ptr[i]^ptr[j];
                i++;
                j--;
        }
        return NULL;
}
int  main(){
        pthread_t t1;

        char *str=malloc(200);
        if(str==NULL){
                printf("Memory allocation failed.\n");
                exit(1);
        }

        printf("Enter string: ");
        fgets(str,200,stdin);
        str[strlen(str)-1]='\0';

        if(pthread_create(&t1,NULL,reverse,str)!=0){
                perror("Failed thread.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        printf("Reversed string: %s\n",str);
        free(str);
}
```
# 51. Write a Program to create thread perform addition of matrices.
## Source Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

typedef struct {
    int row, col;
    int **A, **B, **C;
} ElementData;

void *add_element(void *arg) {
    ElementData *data = (ElementData *)arg;
    int r = data->row;
    int c = data->col;
    data->C[r][c] = data->A[r][c] + data->B[r][c];
    return NULL;
}

int main() {
    int m, n;
    printf("Enter rows and columns of matrices: ");
    scanf("%d %d", &m, &n);


    int **A = malloc(m * sizeof(int *));
    int **B = malloc(m * sizeof(int *));
    int **C = malloc(m * sizeof(int *));
    for (int i = 0; i < m; i++) {
        A[i] = malloc(n * sizeof(int));
        B[i] = malloc(n * sizeof(int));
        C[i] = malloc(n * sizeof(int));
    }


    printf("Enter elements of first matrix:\n");
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &A[i][j]);


    printf("Enter elements of second matrix:\n");
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &B[i][j]);


    pthread_t threads[m][n];
    ElementData edata[m][n];

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            edata[i][j].row = i;
            edata[i][j].col = j;
            edata[i][j].A = A;
            edata[i][j].B = B;
            edata[i][j].C = C;

            if (pthread_create(&threads[i][j], NULL, add_element, &edata[i][j]) != 0) {
                perror("Thread creation failed");
                exit(1);
            }
        }
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            pthread_join(threads[i][j], NULL);
        }
    }

    printf("Resultant matrix (A + B):\n");
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }

    for (int i = 0; i < m; i++) {
        free(A[i]);
        free(B[i]);
        free(C[i]);
    }
    free(A);
    free(B);
    free(C);

    return 0;
}
```
# 52. Write a program create thread a find lenght of the string
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>

void *langth(void *arg){
        char *ptr=(char *)arg;

        int *count=malloc(sizeof(int));
        if(count==NULL){
                printf("Memory alocation failed.\n");
                exit(1);
        }
        *count=0;
        while(*ptr!='\0'){
                *count=*count+1;
                ptr++;
        }
        pthread_exit(count);
}
int main(){
        pthread_t t1;
        char *str=malloc(sizeof(char) * 200);
        if(str==NULL){
                printf("Memory allocation failed.\n");
                exit(1);
        }
        printf("Enter string: ");

        fgets(str,200,stdin);
        str[strlen(str)-1]='\0';
         if(pthread_create(&t1,NULL,langth,str)!=0){
                 perror("Failed thread.\n");
                 exit(1);
         }

         int *len;
         pthread_join(t1,(void **)&len);

         printf("Length of the strig: %d\n",*len);
         free(len);
}
```
# 53. Write a program tp create two threads using pthreadslibrary each thread should print "Hello World!" along with its threead ID?.
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#define ch "Hello World!"

pthread_mutex_t lock;
void *threadfun1(void *arg){

        pthread_mutex_lock(&lock);
        printf("Thread 1: %s and My ID: %lu\n",ch,(unsigned long)pthread_self());
        pthread_mutex_unlock(&lock);
        return NULL;
}

void *threadfun2(void *arg){
        pthread_mutex_lock(&lock);

        printf("Thread 2: %s and my ID: %lu\n",ch,(unsigned long)pthread_self());

        pthread_mutex_unlock(&lock);

        return NULL;
}

int main(){
        pthread_t t1,t2;
        if(pthread_create(&t1,NULL,threadfun1,NULL)!=0){
                perror("Failed thread1.\n");
                exit(1);
        }

        if(pthread_create(&t2,NULL,threadfun2,NULL)!=0){
                perror("Failed thread2.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
}

```
# 54. Modify the previous problem to pass arguments to threads and print those arguments along with the thread ID?
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

typedef struct {
        int a;
        int b;
}Node;
pthread_mutex_t lock;
void *threadfun1(void *arg){
        Node *ptr=(Node *)arg;
        pthread_mutex_lock(&lock);
        printf("Thread 1: sum: %d and My ID: %lu\n",ptr->a + ptr->b,(unsigned long)pthread_self());
        pthread_mutex_unlock(&lock);
        return NULL;
}

void *threadfun2(void *arg){
        Node *ptr=(Node *)arg;

        pthread_mutex_lock(&lock);

        printf("Thread 2: Difference: %d and My ID: %lu\n",ptr->a - ptr->b,(unsigned long)pthread_self());

        pthread_mutex_unlock(&lock);

        return NULL;
}

int main(){
        pthread_t t1,t2;

        pthread_mutex_init(&lock,NULL);
        Node node;
        printf("Enter Numbers: ");
        scanf("%d %d",&node.a,&node.b);
        if(pthread_create(&t1,NULL,threadfun1,&node)!=0){
                perror("Failed thread1.\n");
                exit(1);
        }

        if(pthread_create(&t2,NULL,threadfun2,&node)!=0){
                perror("Failed thread2.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        pthread_mutex_destroy(&lock);
}
```
# 55. Write a C Program to demonstrate thread synchronization using mutex locks. Create two threads that increment a shared variable using mutex to ensure proper synchronization?
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

int glob;
pthread_mutex_t lock;
void *threadfun1(void *arg){
        int n=*(int *)arg;
        for(int i=1;i<=n;i++){
                pthread_mutex_lock(&lock);
                int loc=glob;
                loc++;
                glob=loc;
                pthread_mutex_unlock(&lock);
        }
        return NULL;
}

void *threadfun2(void *arg){
        int n=*(int *)arg;
        for(int i=1;i<=n;i++){
                pthread_mutex_lock(&lock);
                int loc=glob;
                loc++;
                glob=loc;
                pthread_mutex_unlock(&lock);
        }
        return NULL;
}


int main(){
        pthread_t t1,t2;
        int loop=2000;

        pthread_mutex_init(&lock,NULL);
        if(pthread_create(&t1,NULL,threadfun1,&loop)!=0){
                perror("Failed thread1.\n");
                exit(1);
        }

        if(pthread_create(&t2,NULL,threadfun2,&loop)!=0){
                perror("Failed thread2.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);

        printf("glob: %d\n",glob);
        pthread_mutex_destroy(&lock);
}
```

# 56. Extend the previous program to use semaphore insted of mutex locks for thread synchronization?
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<semaphore.h>
int glob;
sem_t lock;
void *threadfun1(void *arg){
        int n=*(int *)arg;
        for(int i=1;i<=n;i++){
                sem_wait(&lock);
                int loc=glob;
                loc++;
                glob=loc;
                sem_post(&lock);
        }
        return NULL;
}

void *threadfun2(void *arg){
        int n=*(int *)arg;
        for(int i=1;i<=n;i++){
                sem_wait(&lock);
                int loc=glob;
                loc++;
                glob=loc;
                sem_post(&lock);
        }
        return NULL;
}


int main(){
        pthread_t t1,t2;
        int loop=2000;

        if(sem_init(&lock,0,1)!=0){
                perror("Semaphore init failed.\n");
                exit(1);
        }
        if(pthread_create(&t1,NULL,threadfun1,&loop)!=0){
                perror("Failed thread1.\n");
                exit(1);
        }

        if(pthread_create(&t2,NULL,threadfun2,&loop)!=0){
                perror("Failed thread2.\n");
                exit(1);
        }

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);

        printf("glob: %d\n",glob);
        sem_destroy(&lock);
}
```
# 57. Write a C Program to impement the producer - consumer problem using threads. Create two threads one for producing items and another for consuming items from a shared buffer?
## Source Code

```c

#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<unistd.h>
#define BUFFER_SIZE 5

int buff[BUFFER_SIZE];

int in=0,out=0,count=0;

pthread_mutex_t lock;
pthread_cond_t no_full,no_empty;
void *producer(void *arg){
        int item=1;
        while(1){
                pthread_mutex_lock(&lock);
                while(count==BUFFER_SIZE){
                        printf("Producer wait, buffer full......\n");
                        pthread_cond_wait(&no_full,&lock);
                }

                buff[in]=item;
                printf("Produced: %d at index: %d\n",item,in);
                count++;
                in=(in + 1) % BUFFER_SIZE;
                item++;

                pthread_cond_signal(&no_empty);
                pthread_mutex_unlock(&lock);
                sleep(1);
        }
        return NULL;
}

void *consumer(void *arg){
        while(1){
                pthread_mutex_lock(&lock);

                while(count==0){
                        printf("Consumer wait, buffer empty.....\n");
                        pthread_cond_wait(&no_empty,&lock);
                }

                int item=buff[out];
                printf("Consumer: %d at index: %d\n",item,out);
                out=(out+1) % BUFFER_SIZE;

                count--;

                pthread_cond_signal(&no_full);
                pthread_mutex_unlock(&lock);

                sleep(2);
        }
        return NULL;
}

int main(){
        pthread_t t1,t2;

        pthread_mutex_init(&lock,NULL);
        pthread_cond_init(&no_full,NULL);
        pthread_cond_init(&no_empty,NULL);

        pthread_create(&t1,NULL,producer,NULL);
        pthread_create(&t2,NULL,consumer,NULL);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);

        pthread_mutex_destroy(&lock);
        pthread_cond_destroy(&no_full);
        pthread_cond_destroy(&no_empty);
}
```
# 58. Implement a multithreaded file copy program in C. Create multiple threads to read from one file and write another file concurrently?
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<pthread.h>
#include<unistd.h>

#define threads 4
typedef struct {
        int sfd;
        int dfd;
        int start;
        int size;
}Node;


void *threadfun(void *arg){

        Node *data=(Node *)arg;

        char buffer[1024];

        long int byte_left=data->size;
        long int offset=data->start;

        while(byte_left > 0){
                long int bytes_to_read=(byte_left>1024)?1024:byte_left;

                long int bytes_read=pread(data->sfd,buffer,bytes_to_read,offset);
                if(bytes_read<0)
                        break;

                long int bytes_write=pwrite(data->dfd,buffer,bytes_read,offset);
                if(bytes_write != bytes_read){
                        printf("Write error.\n");
                        break;
                }

                byte_left-=bytes_to_read;
                offset+=bytes_to_read;
        }
        return NULL;
}
int main(int argc,char *argv[]){
        if(argc!=3){
                printf("Usage: %s <source file> <destination file>\n",argv[0]);

                return 1;
        }

        int fd1=open(argv[1],O_RDONLY);

        if(fd1<0){
                printf("Open source file error.\n");
                exit(1);
        }

        int fd2=open(argv[2], O_WRONLY | O_CREAT | O_TRUNC ,0640);

        if(fd2<0){
                printf("Open destination file error.\n");
                close(fd1);
                return 1;
        }

        long int size=lseek(fd1,0,SEEK_END);

        lseek(fd1,0,SEEK_SET);

        int chunk=size/threads;
        int extra=size%threads;

        pthread_t t[threads];
        Node node[threads];

        for(int i=0;i<threads;i++){
                node[i].sfd=fd1;
                node[i].dfd=fd2;
                node[i].start=i*chunk;
                node[i].size=chunk;

                if(i==threads-1){

                        node[i].size+=extra;
                }

                pthread_create(&t[i],NULL,threadfun,&node[i]);
        }
        for(int i=0;i<threads;i++){
                pthread_join(t[i],NULL);
        }



        close(fd1);
        close(fd2);

        printf("File Copied successfully.\n");
}
```
# 59. Write a C program to demonstrate thread cancellation. Create a thread that runs an infinite loop and cancels it after a certain condition is met from the main thread?
## Source Code

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<unistd.h>
void *threadfun(void*arg){
        printf("Thread function starts:\n");

        while(1){
                printf("Thread 1 executing.\n");
                sleep(1);
                //pthread_testcancel();
        }


        return NULL;
}

int main(){
        pthread_t t1;
        if(pthread_create(&t1,NULL,threadfun,NULL)!=0){

                perror("failed thread create.\n");
                return 1;
        }

        sleep(5);

        pthread_cancel(t1);
        pthread_join(t1,NULL);
        printf("Thread execution completed.\n");
}
```
# 61. C program create thread that prints the even numbers between 1 to 20
## Source Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void* printEven(void* arg) {
    printf("Even numbers between 1 and 20 are:\n");
    for (int i = 1; i <= 20; i++) {
        if (i % 2 == 0) {
            printf("%d ", i);
        }
    }
    printf("\n");
    pthread_exit(NULL);
}

int main() {
    pthread_t tid;

    if (pthread_create(&tid, NULL, printEven, NULL) != 0) {
        printf("Error: Failed to create thread.\n");
        return 1;
    }

    pthread_join(tid, NULL);

    printf("Main thread finished.\n");
    return 0;
}
```
# 62. Develop a C program to create two threads that print odd and even numbers alternately?
## Source Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

pthread_mutex_t lock;
pthread_cond_t cond;
int turn = 0; // 0 -> even's turn, 1 -> odd's turn

void* printEven(void* arg) {
    for (int i = 2; i <= 20; i += 2) {
        pthread_mutex_lock(&lock);
        while (turn != 0) {
            pthread_cond_wait(&cond, &lock);
        }
        printf("%d ", i);
        turn = 1;
        pthread_cond_signal(&cond);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

void* printOdd(void* arg) {
    for (int i = 1; i < 20; i += 2) {
        pthread_mutex_lock(&lock);
        while (turn != 1) {
            pthread_cond_wait(&cond, &lock);
        }
        printf("%d ", i);
        turn = 0;
        pthread_cond_signal(&cond);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;

    pthread_mutex_init(&lock, NULL);
    pthread_cond_init(&cond, NULL);

    pthread_create(&t1, NULL, printEven, NULL);
    pthread_create(&t2, NULL, printOdd, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);
    pthread_cond_destroy(&cond);

    printf("\n");
    return 0;
}
```
