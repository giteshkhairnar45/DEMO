Producer-Consumer Problem
Frist command 
nano producer_consumer.c

#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define BUFFER_SIZE 5

int buffer[BUFFER_SIZE];
int in = 0;
int out = 0;

// Semaphores
sem_t empty;
sem_t full;

// Mutex
pthread_mutex_t mutex;

// Producer function
void *producer(void *arg)
{
    int item;

    for (item = 1; item <= 10; item++)
    {
        // Wait if buffer is full
        sem_wait(&empty);

        // Lock buffer
        pthread_mutex_lock(&mutex);

        // Add item to buffer
        buffer[in] = item;
        printf("Producer produced: %d\n", item);

        in = (in + 1) % BUFFER_SIZE;

        // Unlock buffer
        pthread_mutex_unlock(&mutex);

        // Increase full count
        sem_post(&full);

        sleep(1);
    }

    return NULL;
}

// Consumer function
void *consumer(void *arg)
{
    int item;

    for (int i = 1; i <= 10; i++)
    {
        // Wait if buffer is empty
        sem_wait(&full);

        // Lock buffer
        pthread_mutex_lock(&mutex);

        // Remove item from buffer
        item = buffer[out];
        printf("Consumer consumed: %d\n", item);

        out = (out + 1) % BUFFER_SIZE;

        // Unlock buffer
        pthread_mutex_unlock(&mutex);

        // Increase empty count
        sem_post(&empty);

        sleep(2);
    }

    return NULL;
}

int main()
{
    pthread_t producer_thread, consumer_thread;

    // Initialize semaphores
    sem_init(&empty, 0, BUFFER_SIZE);
    sem_init(&full, 0, 0);

    // Initialize mutex
    pthread_mutex_init(&mutex, NULL);

    // Create producer and consumer threads
    pthread_create(&producer_thread, NULL, producer, NULL);
    pthread_create(&consumer_thread, NULL, consumer, NULL);

    // Wait for threads
    pthread_join(producer_thread, NULL);
    pthread_join(consumer_thread, NULL);

    // Destroy semaphores and mutex
    sem_destroy(&empty);
    sem_destroy(&full);
    pthread_mutex_destroy(&mutex);

    return 0;
}

Complie and run 
gcc producer_consumer.c -o producer_consumer -pthread

Sample output 
Producer produced: 1
Consumer consumed: 1
Producer produced: 2
Producer produced: 3
Consumer consumed: 2
Producer produced: 4
Producer produced: 5
Consumer consumed: 3
Producer produced: 6
Consumer consumed: 4
...