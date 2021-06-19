# Move Semantics

Cpp has been helping simplify the code of objects. When we have a defined vector v1, a just-declared empty vector v2 and `v2=v1`, the result will be v2 has all the copied value of v1 but not the address. In that case, we won't need to consider the issue of sharing. 

The only exception comes from assignment of a return value. When we have a function return the vector, we know the vector will be temporary if we don't store it. Copying all the values to another memory address might be unnecessary. We want to just copy the pointer. Another similar case is `v1 = std::move(v2)`, when we only wish to copy the address pointer.

How does the system know which case = is dealing with? CPP has different types for the RHS of assignment L value, R value and X value. L value is the value we stores after the assignment, the R value is the value might be deleted and X value is created for this special case that is not only used for passing values but also reserving the old variable (although it is not recommended to reuse the old, like after destructor is called).

