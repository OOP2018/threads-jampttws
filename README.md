## Threads and Synchronization

This lab illustrates the problem of synchronization when many threads are operating on a shared object.  The general issue is called "thread safety", and it is a major cause of errors in computer software.

## Assignment

To the problems on the lab sheet and record your answers here.

1. Record average run times.
2. Write your explanation of the results.  Use good English and proper grammar.  Also use good Markdown formatting.

## ThreadCount run times

These are the average runtime using 3 or more runs of the application.
The Counter class is the object being shared by the threads.
The threads use the counter to add and subtract values.

| Counter class           | Limit              | Runtime (sec)   |
|:------------------------|:-------------------|-----------------|
| Unsynchronized counter  |     1,000,000      |    0.016922     |
| Using ReentrantLock     |     1,000,000      |    0.128224     |
| Syncronized method      |     1,000,000      |    0.086012     |
| AtomicLong for total    |     1,000,000      |    0.040809     |

## 1. Using unsynchronized counter object

1.1 - When I run the program for several times it has one time that total is zero.
    - The total is not the same in each times.
    
1.2 - for 10,000,000
      |    1    |  0.030938 sec. | 
      |    2    |  0.026058 sec. |  
      |    3    |  0.027754 sec. | 
      | average |  0.2825 sec.   |

1.3 - The counter total sometimes not equal to zero because the program has two threads running at the same       time with the same counter. Then when one thread had run already it will return the total back to counter and the others get that total to run, so the total will not be zero.   

## 2. Implications for Multi-threaded Applications

How might this affect real applications?  
- In  backing application, it will cause the effect like if you transfer a money to your bank account by mobile banking and you withdraw by ATM in the same time, the value will not correct. 

## 3. Counter with ReentrantLock

3.1 - Yes, the total always be zero. average runtime is 0.128224 second.

3.2 - It difference from problem 1 because this thread has a Lock and the total always be zero. 

3.3 - ReentrantLock will lock the thread, like if you run the thread the counter will finished first thread already then done the second. Show that two threads cannot run in the same time, the total in the program will equal zero.

3.4 - The code in "finally" must be run, even it catch or not. We write "finally { lock.unlock(); }" because the Lock must unlock then the next thread will be run.

## 4. Counter with synchronized method

4.1 - Yes, the total equals zero. average runtime is 0.086012 second.

4.2 - It difference from problem 1 because the total always be zero and the threads cannot run add method in the same time.

4.3 - "Synchronized" means that if you create synchronized method, that method cannot call in the same time. We use synchronize because we don't want anyone call the same method.

## 5. Counter with AtomicLong

5.1 - If use AtomicCounter the thread will get the data from counter and run it normally then compare the value that we get with the actual value, if the actual value does not matches the value that we use for the computation then re-compute it with the new value, else if the values is match then everything is fine.

5.2 - We would AtomicLong when the data is not too complicated.

## 6. Analysis of Results

6.1 - AtomicCounter is fastest and ReentrantLock is slowest.

6.2 - ReentrantLock because we can choose the scope of what we are locking. For example, if we have a complicated program that does many things and only a part will cause side effect when multiple threads run at the same time, so we want to lock only that part. Synchronized Method lock the whole thing. AtomicCounter cannot work in complicate code.

## 7. Using Many Threads (optional)

