# Pointers and Memory

##Smart Pointers

### std::unique_ptr

Unique pointer is a replacement for the deprecated std::auto_ptr in c++ 98. Both of them declare an exclusive ownership to the object. The only difference is that uniqle_ptr can not be copied and auto_ptr supports the copy operation, which in fact, under the hood, is a moving process. This causes quite many errors and is also probably why this feature is no longer used.
There is no overhead in space and time compared to a raw pointer.
`Unique_ptr.reset()` will deletes the old resource from the unique_ptr. We can also initialize the unique_ptr by `delete:std::unique_ptr<T,Deleter>` to self define a deleter (destructor in default). In that case, `Uniqle_ptr.get_deleter()` will return the delete.

