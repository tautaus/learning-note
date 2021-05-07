# Pointers and Memory

## Smart Pointers

### std::unique_ptr

Unique pointer is a replacement for the deprecated std::auto_ptr in c++ 98. Both of them declare an exclusive ownership to the object. The only difference is that uniqle_ptr can not be copied and auto_ptr supports the copy operation, which in fact, under the hood, is a moving process. This causes quite many errors and is also probably why this feature is no longer used.

There is no overhead in space and time compared to a raw pointer.

`Unique_ptr.reset()` will deletes the old resource from the unique_ptr. We can also initialize the unique_ptr by `delete:std::unique_ptr<T,Deleter>` to self define a deleter (destructor in default). In that case, `Uniqle_ptr.get_deleter()` will return the deleter.

Need to be cautious about declare multiple unique pointers over the same object. That might cause UB.

### std::shared_ptr

Shared_ptr shares a resource and manages its lifetime by the reference and reference counter. It’s like what happened in python/java ---- saves the resource and deletes only when no pointer points to it. It has more or less overhead on time and space. It is “more” because its mechanism brings more operation and “less” when there are much more pointers than the objects.

Is the shared_ptr thread-safe? The answer is “it depends”. The control block of shared_ptr is thread-safe, but the underlying resources are not.

In one hand, objects used in different threads are usually independent to each other, which is a contradiction to the conception of shared_ptr. But on the other hand, shared_ptr could save more memory space and better than that, it knows the who the last owner to the resource is, which the thread itself doesn't, so the deallocation process could be handled well. In fact, shared_ptr is the only data type in C++11 which could use atomic operations. In C++20, we will have atomic_shared_ptr, which will also guarantee a thread safe resource.

Same as unique_ptr, we could self-define a deleter (with some logic inside) to keep the information even when some resources are deleted.

### std::weak_ptr

First of all, weak_ptr is not a classic smart pointer because it DOESN'T own and CAN'T access a resource. It can only do two things:
- weak_ptr.expired() to check if the resource exists
- weak_ptr.lock() to create a new shared_ptr to the resource (if available)

Other than a pointer, it looks more like a pointer to poiner, but why do we need a smart pointer for that?

To solve the cyclic reference problem. Suppose we have node_1 and node_2, they are initialized with shared_ptr's linked to each other by shared_ptr's. When they go out of the scope, no one will be deleted and we will have a memory leak! Here is what happened:
1. The shared_ptr that points to the node_1 goes out of the scope first and checks if there are other shared_ptr's point to node_1.
2. There is a shared_ptr of node_2 that points to node_1, so node_1 will not be deallocated and the shared_ptr of node_1 that points to node_2 stays.
3. Same thing happens to node_2, and everything stays.

Such situation will not happen if we use shared_ptr as the forward link and weak_ptr as the backward link. In specific, the above-mentioned step 2 won't happen as weak_ptr IS NOT a owner to the resource.



