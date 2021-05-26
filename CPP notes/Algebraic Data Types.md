# Algebraic Data Types

There are 4 types of algebraic data type in std library:

- std::pair, since C++ 98, the original algebraic data type.
- std::tuple, since C++ 11
- std::optional, since C++ 17
- std::variant, since C++ 17, and C++ 20 fixes its constructor bug.

## Basic Introduction

Pair and Tuple are product type. The product type binds elements together. There might be some padding spaces between elements with different memory size. The order of elements aligned in the product type differs between different libraries. 

The # of possible values of a product type is the product of the # of possible values. The # of possible values of a `variant` object is the sum of the # of possible values. Variant is like a union of types, for example, `variant<A,B>` can be a value of type A or type B. Hence, the "CONCEPTUAL" memory size of a variant will be the max of size of its alternatives. However, if the variant only remember the max size, it will have no idea about what type the alternative is. Therefore, we will need a index part for variant to remember the data type.

The # of possible values of an `optional` object equals to the # of possible values plus 1. `optional<A>` is similar to `variant<A,null>`.

### Motivation

`Pair` was wildly used in classic STL. In the `std::map`, each entry is a pair of key and value. When we try to insert a pair into the map, the pair of iterator and bool that represents the success of the insertion will be returned. 

`Tuple` is a multi-variate case of `pair` when people need more than 2 results. 

`Optional` is not used in STL. It tells the caller that the function might result "no result". We can use `.value_or` to shorten the code of checking the flag. 

### Terminology 

*Engage* and *disengage* are only relevant to `optional`. An `optional` holds a value is said to be engaged, and vice versa. As mentioned above, we use `has_value` to test it.

We can treat variant or optional as a buffer that may or may not hold a value so that we can *emplace* an object into them. When emplace a new object, the old value will be deleted if exists.  

While constructing a variant or optional with an object already emplaced, we can simply use `=` to implicitly move the objects. However, when the object is not movable, we will have to use in_place construction to let the constructor to forward the argument to the constructor. For example, `optional<string> o(std::in_place, “abc”)` or `variant<int, string> v(std::in_place_type<std::string>, “abc”)`.

