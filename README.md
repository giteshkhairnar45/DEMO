Mutex Program
nano mutex.c

#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

pthread_mutex_t mutex;

void *process(void *arg)
{
    int id = *(int *)arg;

    // Lock mutex
    pthread_mutex_lock(&mutex);

    // Critical Section
    printf("Process %d entered the critical section\n", id);
    sleep(2);
    printf("Process %d is working...\n", id);

    // Unlock mutex
    pthread_mutex_unlock(&mutex);

    printf("Process %d left the critical section\n\n", id);

    return NULL;
}

int main()
{
    pthread_t t1, t2;
    int id1 = 1, id2 = 2;

    // Initialize mutex
    pthread_mutex_init(&mutex, NULL);

    // Create threads
    pthread_create(&t1, NULL, process, &id1);
    pthread_create(&t2, NULL, process, &id2);

    // Wait for threads
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    // Destroy mutex
    pthread_mutex_destroy(&mutex);

    return 0;
}

Complie and run 
gcc mutex.c -o mutex -pthread
./mutex

Sample output 
Process 1 entered the critical section
Process 1 is working...
Process 1 left the critical section

Process 2 entered the critical section
Process 2 is working...
Process 2 left the critical section