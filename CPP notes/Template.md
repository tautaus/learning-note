# Template

Template is a kind of pattern that compiler fills the value out.

## Template Types

### Function-Template

In the instantiation, the compiler replaces a template with specific arugments. C++ Insigts could help to visualize the result. The compiler instantiates the function template when it can derive from arguments.

Specialization is the procedure that provides a concrete implementation for an argument combination of a function template. It sets a special case that overwrites the template. Using `template<> <same_function_signature>(specific type)` would do.

### Class-Template

Similar to Function-Template, class template is introduced by the keyword *template*. The class method can be implemented inside or outside. The one implemented outside will require the `template` head before the the method. Class Template could also use none-type parameter as inputs such as size_t as the size of array.
