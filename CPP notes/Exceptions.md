# Exceptions

Exceptions are the error handling models that are required by the key parts of the language. However, it is wildly banned not only in projects but guidelines. Large portion of developers are using non-standard libraries or language dialect instead.

There are plenty of reason:
- Performance overhead: throwing exception could be very runtime consuming (100x) than other operations from various compilers.
- Harder Reasoning
- Rely on dynamic memory: it is a big deal breaker when dynamic allocation is not allowed
- Increase the binary size: the catcher code will be stored somewhere in the binary file by the compiler

These problems can be solved technically.

## How do exceptions work

