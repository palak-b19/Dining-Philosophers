# Dining-Philosophers

## Introduction:

The Dining Philosophers Problem is a classic synchronization challenge in computer science, designed to explore the intricacies of resource allocation and deadlock avoidance in a multi-threaded environment. This problem simulates a dining table with a fixed number of philosophers, each engaged in two activities: thinking and eating. To dine, a philosopher must acquire two adjacent forks. The challenge is to design a solution that ensures mutual exclusion, prevents deadlocks, and provides a fair opportunity for each philosopher to eat.

## Problem Description:

The core challenge in the Dining Philosophers Problem stems from the potential for circular waiting. If each philosopher attempts to acquire the left fork and then the right fork, a deadlock may occur as each philosopher holds one fork and waits for the other, creating a circular dependency.

## Code Overview:

### Header Files and Libraries:
c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <time.h>


These standard C libraries include thread management (pthread.h), input/output (stdio.h), sleeping (unistd.h), dynamic memory allocation (stdlib.h), and time functions (time.h).

### Constants and Macros:
c
#define Total_Philosophers 5
#define Total_Bowls 2
#define Max_Iterations 1000


These define the total number of philosophers, total number of bowls, and the maximum number of thinking-eating cycles for each philosopher.

### Global Variables:
c
pthread_mutex_t forks[Total_Philosophers];
pthread_mutex_t bowls[Total_Bowls];
pthread_cond_t fork_available[Total_Philosophers];
pthread_cond_t bowl_available[Total_Bowls];


These global variables include mutexes for forks and bowls, as well as condition variables to signal the availability of forks and bowls.

### Helper Functions:
c
int left_fork(int philosopherNumber);
int right_fork(int philosopherNumber);
int bowl_id(int philosopherNumber);


These functions calculate the index of the left fork, right fork, and associated bowl for a given philosopher.

### Eating and Thinking Functions:
c
void eating(int philosopherNumber);
void thinking(int philosopherNumber);


These functions simulate the actions of eating and thinking, printing messages and introducing random sleep to mimic the time spent on each activity.

### Resource Management Functions:
c
void get_forks(int philosopherNumber);
void put_forks(int philosopherNumber);


These functions handle the acquisition and release of forks. get_forks locks the left and right forks, and put_forks unlocks them, signaling their availability.

### Bowl Selection Function:
c
int chooseBowl();


This function selects a bowl, attempting to lock each bowl and returning the index of the first available bowl. If all bowls are taken, it waits for a signal.

### Philosopher Thread Function:
c
void* philosopher(void* args);


This is the main function that represents the behavior of each philosopher. It includes a loop for thinking, acquiring forks and bowls, eating, and releasing resources.

### Main Function:
c
int main();


The main function initializes the resources (forks and bowls), creates philosopher threads, waits for their completion, and cleans up resources.

## Code Execution:

The program starts by initializing mutexes and condition variables for forks and bowls. It then creates philosopher threads, each represented by the philosopher function. The threads execute the thinking, eating, and resource management routines.

The key aspects of the code execution include:

- *Deadlock Mitigation:*
    The introduction of bowls as an additional shared resource breaks the circular waiting condition, mitigating the potential for deadlocks.

- *Fairness Mechanism:*
    Condition variables and the pthread_cond_wait function are used to promote fairness. When a philosopher cannot acquire a bowl, they wait on the condition variable associated with the left fork. This ensures that, over time, all philosophers have a fair chance to eat.

- *Randomness in Thinking and Eating Times:*
    The use of usleep(rand() % 1000000) introduces randomness in thinking and eating times, simulating a more realistic scenario where access to resources is not perfectly synchronized.

- *Thread Synchronization:*
    The program uses mutexes to control access to shared resources, ensuring that only one philosopher can acquire a fork or bowl at a time. Condition variables are used to notify waiting philosophers when a resource becomes available.

## Building and Running:

To compile the program, use a C compiler such as gcc:

bash
gcc dining_philosophers.c -o dining_philosophers -lpthread


Run the executable:

bash
./dining_philosophers


## Conclusion:

The provided solution serves as a comprehensive example of addressing the Dining Philosophers Problem in a concurrent programming environment. It illustrates practical strategies for deadlock mitigation and fairness promotion in resource allocation. This code is a valuable learning resource for those seeking a deeper understanding of synchronization mechanisms in multi-threaded applications.
```
