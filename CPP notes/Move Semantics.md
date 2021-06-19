# Move Semantics

## Basics

Cpp has been helping simplify the code of objects. When we have a defined vector v1, a just-declared empty vector v2 and `v2=v1`, the result will be v2 has all the copied value of v1 but not the address. In that case, we will not need to consider the issue of sharing. 

The only exception comes from assignment of a return value. When we have a function return the vector, we know the vector will be temporary if we don't store it. Copying all the values to another memory address might be unnecessary. We want to just copy the pointer. Another similar case is `v1 = std::move(v2)`, when we only wish to copy the address pointer.

How does the system know which case = is dealing with? CPP 11 has different types for the RHS of assignment L value, R value and X value. L value is the value we stores after the assignment, the R value is the modifiable objects that are no longer needed after destructor is called).

The inside of `std::move` is surprisingly simple: `return static_cast<std::move_reference_t<T>&&>(t)`. So here comes the old saying, "std::move doesn't move anything". It only unconditionally casts the input into an rvalue reference and leaves all the checks to static_cast.

## Member Functions

In the move constructor, it is recommended to apply std::move on all the member variables, espcially on those which are not default type. It is also recommended to include `noexcept` on it as moves are already noexcept and it saves lots of runtime. 

Ideally, the moved-from should be the default value of the type unless there is an exceptionally good reason not to do so, for example, some types might not have default values or establishing the default value is expensive. There is a simplification of resetting to default value: `l.ptr = std::exchange(r.ptr, nullptr)`.

In the move assignment operation, the cleanup will be done followed by all the other operations in the constructor.

The default move operations are generated unless copy operation or destructor is user-defined. The default copy operations are generated unless move operation is user-defined.

## Forwarding References

The forwarding references are the references whose reference types are determined by the passed in reference (as passing value could be awfully expensive not to mention some objects are not copiable).

When pass in the wrong references, the code will not compile. We could rigorously limit the argument with `const` as lvalue, but it will introduce unnecessary overhead. Therefore, we use `std::forward`.

Like std::move, forward also has a one-line body: `static_cast<T&&>(t)`. The core mechanism here is reference collapsing. Developers are not allowed to write code with multiple references stacked together but in some special cases like template (when the replacement is very direct), compiler will collapse the references together, all combinations will be collapsed to a lvalue reference unless all the references are rvalue reference. 

"std::forward doesn't forward anything". Different from move, forward keeps the reference type same. 

### Overloading with Forwarding References

Now we have 6 types of input: lvalue, const lvalue, rvalue, const rvalue, T &&, const T &&. 
- when f(x) is called, lvalue is preferred than T && as nontemplate is preferred
- when f(get_x()) is called, the priority will be
  1.  rvalue, the perfect fit
  2.  T&&, as template has lower priority
  3.  const rvalue
  4.  const T&& (but const consume time)
  5.  const lvalue (the very last fallback)
- when f(const get_x()) is called, the priority will be
  1.  const lvalue
  2.  const T&&
  3.  T &&
  4.  const lvalue

