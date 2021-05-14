# Concurrency

Back to standard C++ 03, the language does not have any concept of concurrency and thread. There are only some platform-specific libraries which only provide some basic functionalities to create multi threads in the OS.

To support the multi-thread mode, we need to consider if the compiler or hardware can improve/reorder the code. If yes, will they know if they are changing the variable that other thread might access?

## Memory Model

C++ 11 provides us a formal model of a multi-thread program. Each thread has the right to access the memory. In the multi-thread mode, each write to an address must `synchronize-with` other reads or writes, or else causes a UB. These `synchronize-with` relationships can be established by using some standard libraries such as std::mutex and std::atomic<T>.

In C++ 03, we use calling the third-party function (like pthread) to create a new thread. In C++ 11, we can easily create a std::thread object by writing the callable as the constructor argument. The thread starts immediately and when it finishes it becomes *joinable*. Calling `join()` on a std::thread object will block (at any thread) before the thread is destroyed. 

`Join()` does not prevent data race if multiple threads try to change a object with a regular data type. That’s why we need to use std::atomic<T>. Every read/write operation to the atomic variable is synchronized with others. Though the unknown order of the operations could still be a bug in the real life, they will not cause UB.

`Join()` allows us to block the code in a thread until certain thread is finished, but can we do it reversely that allow a thread to wait until unblock called by the other thread. Here is a non-solution, busy-waiting:

Create a shared variable and use it as the condition of a while loop that blocks the code. When another thread changes the variable, the while loop exits, and the code is unblocked. Although the code after the while loop won't be executed, the thread is still working. It is a waste of power and time (if on a single-core machine). Besides, compiler might find out the ready in the loop never changes and cause UB.

One way to solve the problem is to use std::mutex, a mutual exclusion mechanism. It has 2 methods: lock and unlock. When a thread tries to lock a mutex, it will either be blocked if the mutex is locked or continue and lock the mutex of the mutex is not locked. 

We can use mutex to protect the thread safety by putting the code in between mutex.lock() and mutex.unlock(). However, if other piece of code has access to the memory used in between the 2 mutex calls, there will still be UB’s. Hence, the protection must be complete. There is a cleaner way instead of calling lock and unlock around the function, a local std::lock_guard<std::mutex>. The std::mutex will be used as an argument of its constructor and be locked. Similarly, lock_guard’s destructor will be called the function ends and unlock the mutex. 
