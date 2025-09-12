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
