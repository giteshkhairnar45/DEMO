#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

void *task1(void *arg)
{
    for (int i = 1; i <= 5; i++)
    {
        printf("Thread 1: %d\n", i);
        sleep(1);
    }
    return NULL;
}

void *task2(void *arg)
{
    for (int i = 1; i <= 5; i++)
    {
        printf("Thread 2: %d\n", i);
        sleep(1);
    }
    return NULL;
}

int main()
{
    pthread_t t1, t2;

    pthread_create(&t1, NULL, task1, NULL);
    pthread_create(&t2, NULL, task2, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Both threads completed.\n");

    return 0;
}





First command 
nano concurrency.c 


Second command 
gcc concurrency.c -o concurrency -pthread

3rd command
./concurrency

Sample output 

Thread 1: 1
Thread 2: 1
Thread 1: 2
Thread 2: 2
Thread 1: 3
Thread 2: 3
Thread 1: 4
Thread 2: 4
Thread 1: 5
Thread 2: 5
Both threads completed.



Mutual exclusive 

#include <stdio.h>
#include <pthread.h>

int counter = 0;

pthread_mutex_t mutex;

void *increment(void *arg)
{
    for (int i = 0; i < 5; i++)
    {
        pthread_mutex_lock(&mutex);

        counter++;
        printf("Thread %ld: Counter = %d\n", (long)arg, counter);

        pthread_mutex_unlock(&mutex);
    }

    return NULL;
}

int main()
{
    pthread_t t1, t2;

    pthread_mutex_init(&mutex, NULL);

    pthread_create(&t1, NULL, increment, (void *)1);
    pthread_create(&t2, NULL, increment, (void *)2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&mutex);

    printf("Final Counter = %d\n", counter);

    return 0;
}

Frist command 
nano mutex.c

Second command 
gcc mutex.c -o mutex -pthread

3rd command 
./mutex

Sample output 
Thread 1: Counter = 1
Thread 1: Counter = 2
Thread 2: Counter = 3
Thread 2: Counter = 4
Thread 1: Counter = 5
Thread 2: Counter = 6
Thread 1: Counter = 7
Thread 2: Counter = 8
Thread 1: Counter = 9
Thread 2: Counter = 10

Final Counter = 10


Bash command 
gcc mutex.c -o mutex -pthread
./mutex# DEMO
a
