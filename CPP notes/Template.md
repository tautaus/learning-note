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

When inheriting from a class template, the derived class does not have methods and attributes automatically. This is because when we have multiple instantiations about the base class, the compiler will not know which one to call. There are 2 alternative solutions for that 
-	`this->func()`
-	`Base<T>::func()`

Alias templates allow people to create synonyms for the templates. Using the previous example of array `Array<T, size>`, using `CharArray = Array<char,N>` would make a easier for CharArray. One thing we need to be cautious about is that Alias templates CANNOT be further specialized. 

### Variadic Template

In C++ 11, variadic template allows uncertain number of arguments, which is also called parameter pack . `typename… Ts` ‘declares’ the pack of arguments that can have arbitrary length. In the template, `Ts… ts` is the function argument that contains the pack,`sizeof…(ts)` returns the number of arguments passed and `ts…` unpack the pack of arguments.

Besides the recursion, we could also use the fold expression to unpack the parameter pack.  When we have a pack to be passed into an operation:
-	Unary operation:
    - Right fold:  (pack *op* …)
    -	Left fold: (… *op* pack)
-	Binary operation:
    -	Right fold: (pack *op* .. *op* init)
    - Left fold: (init *op* .. *op* pack)

Note that the parenthesis is requited for the fold expression and init stand for initial value when there is only 1 argument for binary operation.

## Debug

### Substitution Failure Is Not An Error

When the compiler fails at instantiation, it could silently ignore the failure. If in the end, the compiler could not find a match function, it throws a compiler error, which will warn us about the bug. 

Substitution Failure Is Not An Error (SFINAE) is a technique designed for this moment. It simplifies the case more than another specialization or overload does. By having `std::enable_if_t<std::is_sometype_v<T>, bool>` between the template head and function signature of an existed specialization, the compiler will match the function to <some_type>.

### Tag Dispatch

Like SFINAE, tag dispatch is another solution for instantiation failures. We pass a *class tag* into the function (without type declaration). For example:

`function(T t1, T t2, typeA) and function(T t1, T t2, notTypeA)`

where typeA and notTypeA is declared in the internal namespace. The real function `function(T t1, T t2)` calls the internal namespace and checks the type of T to determine which one of typeA and notTypeA will be passed in.
