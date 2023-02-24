---
title: "Primitives in C++ for Multithreading"
date: 2023-02-23T19:29:20-05:00
draft: false
tags: [multithreading, C++]
showToc: true
---

Following is a short and concise overview of the synchronisation primitives provided in the C++ STL (C++ 20 and above). 

## std::mutex 
1. allows a thread to modify a shared variable.
    
    [std::mutex](https://en.cppreference.com/w/cpp/thread/mutex)
    
2. must be released by the same thread that locks it.
3. other threads who try to acquire the lock are blocked
4. other threads can avoid being blocked by using try_lock. This tries to use a lock, but is not blocked if lock is already taken. This allows the attempting thread to do somthing else while waiting for the lock to release.
5. To achieve mutual exclusion in cpp, you can use std::mutex but an std::atomic also works fine. std::atomic can be used for cpp data types or even shared/weak pointers. 
    
    [std::atomic](https://en.cppreference.com/w/cpp/atomic/atomic)
        
## std::shared_mutex
 1. allows multiple threads shared access if are only going to read from the shared data
 2. writers have exclusive access. When lock is acquired by writers, readers and other writers are blocked.
 3. lock had to be released by the same thread that acquires it.
 
 [std::shared_mutex](https://en.cppreference.com/w/cpp/thread/shared_mutex)
    
## std::scoped_lock
 1. provides a convenient way of owning a mutex through cpp’s RAII style. lock is released when execution goes out of scope.
 2. can hold multiple nested mutexes.
     
     [std::scoped_lock](https://en.cppreference.com/w/cpp/thread/scoped_lock)
        
## std::condition variable
 1. has a queue that holds the waiting threads blocked on some condition on the shared variable. 
 2. Threads are blocked until another thread both modifies the shared condition and notifies the condition_variable.
 3. Requires a mutex to work. specifically std::unique_lock<std::mutex>. One condition variable should work only with single mutex. Whereas, multiple condition variables can share a single mutex. 
 4. Thread that wants to change the shared variable must acquire a mutex, modify the shared variable and then release the mutex, and notify cv. After notifying cv, the cv checks if the condition predicate is true and unblocks a waiting thread if it is true.
 5. the example given in cpp reference doc is good.
     
     [std::condition_variable](https://en.cppreference.com/w/cpp/thread/condition_variable)
        
## std::counting_semaphore, std::binary_semaphore
 1. has a mutex, a cv, and count c. in std::counting_semaphore need only to mention count c for costructing a c_semaphore. Think of this as multiple ports on a charger.
 2. a binary semaphore is when the counting c is 1. Which allows only one thread to use the shared resource. This differs from a mutex in the sense of ownership. Mutex has to be released by the calling thread as it is owned by the calling thread. A binary_sempahore can be released by other threads even if they are not the owner of that lock.
 3. Used mainly for signalling/notifying rather than mutual exclusion
 4. is similar to cv in sense that threads wait on the condition of c becoming > 0.
     
     [std::counting_semaphore, std::binary_semaphore](https://en.cppreference.com/w/cpp/thread/counting_semaphore)
        
## std::barriers
 1. Used mainly for thread coordination
 2. considers number of threads to wait for the barrier to release. Barrier is released when c number of threads are blocked on the wait() condition.
 3. Can be reused unlike std::latch
     
     [](https://en.cppreference.com/w/cpp/thread/barrier)
        
## std::latch
 1. Used mainly for thread coordination
 2. similar to barrier but doesn’t require a thread to block itself to signal release of the barrier. Barrier is released when the a thread calls the count_down() function. Calling the count_down() function doesn’t block this thread.
 3. Calling thread can decrement latch multiple times.
     
     [std::latch](https://en.cppreference.com/w/cpp/thread/latch)
        
## Futures (Similar to Promise in js)