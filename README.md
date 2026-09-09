Semaphore Program
Frist command 
nano semaphore.c

#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

sem_t semaphore;

void *process(void *arg)
{
    int id = *(int *)arg;

    // Wait / P operation
    sem_wait(&semaphore);

    // Critical Section
    printf("Process %d entered the critical section\n", id);
    sleep(2);
    printf("Process %d is working...\n", id);

    // Signal / V operation
    sem_post(&semaphore);

    printf("Process %d left the critical section\n\n", id);

    return NULL;
}

int main()
{
    pthread_t t1, t2;
    int id1 = 1, id2 = 2;

    // Initialize semaphore with value 1
    sem_init(&semaphore, 0, 1);

    // Create threads
    pthread_create(&t1, NULL, process, &id1);
    pthread_create(&t2, NULL, process, &id2);

    // Wait for threads
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    // Destroy semaphore
    sem_destroy(&semaphore);

    return 0;
}


Complie and run 
gcc semaphore.c -o semaphore -pthread
./semaphore


Sample output 
Process 1 entered the critical section
Process 1 is working...
Process 1 left the critical section

Process 2 entered the critical section
Process 2 is working...
Process 2 left the critical section