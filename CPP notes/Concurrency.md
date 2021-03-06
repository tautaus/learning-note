# Concurrency

Back to standard C++ 03, the language does not have any concept of concurrency and thread. There are only some platform-specific libraries which only provide some basic functionalities to create multi threads in the OS.

To support the multi-thread mode, we need to consider if the compiler or hardware can improve/reorder the code. If yes, will they know if they are changing the variable that other thread might access?

## Memory Model

C++ 11 provides us a formal model of a multi-thread program. Each thread has the right to access the memory. In the multi-thread mode, each write to an address must `synchronize-with` other reads or writes, or else causes a UB. These `synchronize-with` relationships can be established by using some standard libraries such as std::mutex and std::atomic<T>.

In C++ 03, we use calling the third-party function (like pthread) to create a new thread. In C++ 11, we can easily create a std::thread object by writing the callable as the constructor argument. The thread starts immediately and when it finishes it becomes *joinable*. Calling `join()` on a std::thread object will block (at any thread) before the thread is destroyed. 

Data race happens when at least 2 threads access a shared state concurrently and at least one thread tries to modify the shared state. `Join()` does not prevent data race if multiple threads try to change a object with a regular data type. That’s why we need to use std::atomic<T>. Every read/write operation to the atomic variable is synchronized with others. Though the unknown order of the operations could still be a bug in the real life, they will not cause UB.
 
In the compilation, if we include thread sanitizer `-fsanitize=thread` to prevent and even count the data race. This is equivalent to declare every element atomic.

### Mutex Lock

`Join()` allows us to block the code in a thread until certain thread is finished, but can we do it reversely that allow a thread to wait until unblock called by the other thread. Here is a non-solution, busy-waiting:

Create a shared variable and use it as the condition of a while loop that blocks the code. When another thread changes the variable, the while loop exits, and the code is unblocked. Although the code after the while loop won't be executed, the thread is still working. It is a waste of power and time (if on a single-core machine). Besides, compiler might find out the ready in the loop never changes and cause UB.

One way to solve the problem is to use std::mutex, a mutual exclusion mechanism. It has 2 methods: lock and unlock. When a thread tries to lock a mutex, it will either be blocked if the mutex is locked or continue and lock the mutex of the mutex is not locked. 

We can use mutex to protect the thread safety by putting the code in between mutex.lock() and mutex.unlock(). However, if other piece of code has access to the memory used in between the 2 mutex calls, there will still be UB’s. Hence, the protection must be complete. There is a cleaner way instead of calling lock and unlock around the function, a local std::lock_guard<std::mutex>. The std::mutex will be used as an argument of its constructor and be locked. Similarly, lock_guard’s destructor will be called the function ends and unlock the mutex. This also prevents the case when exception throws and the function exits without calling the mutex.unlock.

A good thing about having the mutex lock as an object is that now functions can pass or return the ownership of a lock. There is a unique_lock<mutex> to manage the unique ownership of mutex locks (including lock and unlock). The std::lock_guard is a special case that can't be passed and can only be cleaned. And C++ 17 introduces std::scoped_lock<Ts> that is a lock_guard that takes multiple locks.

### Condition Variable

Whenever we have a “producer” and a “consumer” where the consumer has to wait for the producer and the production and the consumption happen continuously (task queue for example). We could use the combination of mutex lock and condition variable. 

In the consumer side, we can have a lock and a condition variable that waits on the lock. When we call `std::condition_variable.wait()`, the thread will drop the lock and go to sleep atomically, so there will be no gap between 2 actions for others to take the lock. When the thread wakes up again, the wait() will reacquire the lock for the thread that gives the thread access to the resource.
 
In the producer, calling `notify_one`  or `notify_all` methods of condition variable will wake up any/all thread(s) that is blocked on it. There is no big difference between calling the unlock before or after the notify.

### Initialization

What will happen when 2 threads arrive at the initializat ion of static variable concurrently?

C++ 03 leaves lots of UB’s.  Since C++ 11, the static initialization will automatically block the other threads when they arrive after a thread arrives and unblock them after a thread finish. 

When initializing the member variable, we could use the whole mutex lock, but it could slow the whole thing down. C++ 11 provides a std::once_flag. Exactly as its name, if we call 
``std::call_once (once_,[]() {<function_body_here>})``
, the function inside will only be executed once.

## Task

A task is like a data channel, where promise, the data sender, sends the values, exceptions and notifications (to 1 or more receivers) and future, the data receiver calls get and is eventually blocked. Unlike threads, which communicate through shared variable, task is actually the communication chanel for sender and receiver. It can also pass exceptions while all the child and creator threads will be terminated when a exception is caught in a child thread.
 
When a thread goes out of the scope without being joined, `std::terminate` will be called to terminate the whole program. The solution of having the program restored is to add thread.join() in the destructor by hand. C++20 has jthread which automatically implements this functionality. 

