# Template

Template is a kind of pattern that compiler fills the value out.

## Template Types

### Function-Template

In the instantiation, the compiler replaces a template with specific arugments. C++ Insigts could help to visualize the result. The compiler instantiates the function template when it can derive from arguments.

Specialization is the procedure that provides a concrete implementation for an argument combination of a function template. It sets a special case that overwrites the template. Using `template<> <same_function_signature>(specific type)` would do.

### Class-Template

Similar to Function-Template, class template is introduced by the keyword *template*. The class method can be implemented inside or outside. The one implemented outside will require both `template` head before the the method and class name for the compiler to match the class. Class Template could also use none-type parameter as inputs such as size_t as the size of array.

The instantiation of class templates is like that of function template except class template CANNOT automatically derive their arguments. In the other word, each template argument must be specified explicitly. The good thing is that it is no longer a problem after C++ 17 which provides *template argument deduction*. 

Methods of a class template can be a template of their own. For example, a class of template T could define its = operator by any type of input of template U and `static_cast` it. It also can be definite at both inside and outside of the class. There are 2 functions CANNOT be templates: copy constructor and destructor.
