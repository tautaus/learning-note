# Exceptions

Exceptions are the error handling models that are required by the key parts of the language. However, it is wildly banned not only in projects but guidelines. Large portion of developers are using non-standard libraries or language dialect instead.

There are plenty of reason:
- Performance overhead: throwing exception could be very runtime consuming (100x) than other operations from various compilers.
- Harder Reasoning
- Rely on dynamic memory: it is a big deal breaker when dynamic allocation is not allowed
- Increase the binary size: the catcher code will be stored somewhere in the binary file by the compiler

## Exception Practices

These problems above are theoretical problems that could be solved in the next few versions of cpp. The problems we should be careful about in daily coding are different.

### When to Use Exceptions (When not)

*Stack unwinding* is what usually happens after an exception is thrown that every object on the stack will be destroyed in the reverse order of their construction. However, this is not granted. When the try and catch block is omitted, then the exception cannot be kept. When exiting the main function, unhandled exception will result the `std::terminate()` call which brutally terminates the process and leads to memory leak. 

That means, we should use exceptions for errors that:
- are expected to occur rarely
- cannot be dealt locally (like files not found, keys not found in the map)
- for operators and constructors (so that no other mechanism could help)

And we should not use exceptions for errors that:
- might occur frequently, for example, we expect string_to_int function would introduce many errors, the better alternative for exception is to use `std::optional`
- are in functions that we want to guarantee the response time 
- for things should not happen like:
  - dereference the nullptr
  - index out of range 
  - use after free

### How to Use Exceptions

We define and construct errors by std::exception hierarchy. Throw them by Rvalue and catch them by reference to prevent the unnecessary copy.

### The Exception Safety Guarantees

There are kinds of safety guarantees:
- Basic exception safety guarantee: invariants are preserved and no resources are leaked, but variables are not guaranteed to be preserved still.
- Strong exception safety guarantee guarantees no state change on variables (by commit or rollback). It is usually achieved by create a temporary variable. However it is not always possible (in flows like sockets and streams)
- No-Throw guarantee uses `noexcept` to make sure no operation will fail. There are functions that should never fail:

