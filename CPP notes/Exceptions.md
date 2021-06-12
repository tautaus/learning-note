# Exceptions

Exceptions are the error handling models that are required by the key parts of the language. However, it is wildly banned not only in projects but guidelines. Large portion of developers are using non-standard libraries or language dialect instead.

There are plenty of reason:
- Performance overhead: throwing exception could be very runtime consuming (100x) than other operations from various compilers.
- Harder Reasoning
- Rely on dynamic memory: it is a big deal breaker when dynamic allocation is not allowed
- Increase the binary size: the catcher code will be stored somewhere in the binary file by the compiler

These problems above are theoritical problems that could be solved in the next few versions of cpp. The problems we should be careful about in daily coding are different.

*Stack unwinding* is what usually happens after an exception is thrown that every object on the stack will be destoryed in the reverse order of their construction. However, this is not granted. When the try and catch block is omited, then the exception cannot be kept. When exiting the main function, unhandled exception will result the `std::terminate()` call which brutally terminates the process and leads to memory leak. 

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

