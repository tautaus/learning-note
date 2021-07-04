# Lambda

Lambda calculus is a branch of mathematics for expressing computation based on function abstraction and application using variable binding and substitution. In other words, it focuses on if a problem can be solved by a tuning machine or not. It was introduced in 1930s and the usage of computer was extremely  limited. In lambda calculus, the greek letter lambda $\lambda$ was used to define the function without a name and this idea lasts to now.

The definition of a lambda expression was first introduced in C++ 11 as an expression which returns a function object. However, when we declare a lambda experssion, we should only use `auto` because the data type of a lambda expression is compiler dependent.

## Syntax

The syntax for a lambda expression consists of specific punctuation: \[capture clause] (parameter list) -> return type {body}. 

### Capture Clause

The capture clause is the variables which are available inside the body. It is not necessary to capture anything but the square bracket has to be there. We can pass in either value or reference so that a change of the variable after the definition of the lambda can be either visible or invisible in the body. There is another thing we should be cautious of, passed in reference can not be out of the scope before lambda is called.

Everything to the right of the equal sign is the lambda expression. After it is first evaluated, the function object is initialized. The new variable now becomes `closure`.  

When capture the variable by value, the value is copied into the expression as a const value. Only variables in the local scope or `this` can be captured. The capture occurs when the expression is evaluated. We can modify the passed in value by copying non-const value. To do this, we only need to add a `mutable` before the function body.

When reference is captured, an & is added to indicate capture by lvalue reference (means we cannot use rvalue). We cannot capture global variable, but we can use it directly in the body.

In C++ 14, the capture clause was extended to generalized capture. It allows us to copy a variable in the capture clause. This makes capture initialization by `std::move` possible `[var = std::move(out_var)]`. The capture occurs when the expression is evaluated which is the stage when move occurs.

Capture of a class instance is also extended:
- C++ 11 only allows `[this]` to capture this pointer by value (hence every operation is mutable.
- C++ 14 has generalized capture, so `[self=*this]` will capture \*this object by value and initializes a new one
- C++ 17 has the `[*this]` does the same thing as the generalized capture.

There are 2 short forms: `[=]` and `[&]` which represents capture all variables used in the body by value and reference. This expression might be confusing. Starting C++ 20, the default capture of this pointer by value has been deprecated.

After evaluation, if the capture clause is empty, then the C++ will treat it as a special kind of closure, which can be implicitly converted to a function pointer.

### Parameter List

In C++ 11, defualt parameters were not permitted. In C++ 14, with default parameters supported, parameters can have a data type of auto. 
