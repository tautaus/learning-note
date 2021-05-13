# Concurrency

Back to standard C++ 03, the language does not have any concept of concurrency and thread. There are only some platform-specific libraries which only provide some basic functionalities to create multi threads in the OS.

To support the multi-thread mode, we need to consider if the compiler or hardware is allowed to improve/reorder the code. If yes, will they know if they are changing the variable that other thread might access?

## Memory Model

C++ 11 provides us a formal model of a multi-thread program. Each thread has the right to access the memory. In the multi-thread mode, each write to an address must `synchronize-with` other reads or writes, or else causes a UB. These `synchronize-with` relationships can be established by using some standard libraries such as std::mutex and std::atomic<T>.

In C++ 03, we use calling the third-party function (like pthread) to create a new thread. In C++ 11, we can easily create a std::thread object by writing the callable as the constructor argument. The thread starts immediately and when it finishes it becomes *joinable*. Calling `join()` on a std::thread object will block before the thread is destroyed.
