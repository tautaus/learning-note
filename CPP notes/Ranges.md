# Ranges

## Range Basics

The simplest version of *range* is a pair of iterator experssion: {rbegin, rend} that include a range of iterator to do operation with. However, the iterators can be in different types as long as they can compare to each other. 

There are many commonly used tools in the standard library sit in the definition of range. Collections like array, vector, map etc. are ranges. In OS, the directory_iterator and stream iterators are ranges. However, the container adapters like queue and stack are not ranges as they cant be iterated over. 

*Views* are ranges that a re usually defined on another range and transform the underlying range via some range algorithm or operation. In other words, they are "lazy version" of ranges. The important points here are views doesn't own the elements and all the methods are in *O(1)*. That means, views can also be traeted as methods, which makes most containers not views as copying the containers is almost never in constant time.

*Range adaptors* make ranges into views. It create pipelines of lizily evaluated transfomations: `auto filtered_range = range | std::ranges::views::filter( some_filter);`.

The views and range algorithm can be combined together to make codes cleaner. For example, if we have 

`vector <int> v { 6, 2, 3, 4, 5, 6}; auto after_leading_evens = rng::views::drop_while(v, is_even); rng::for_each(after_leading_evens, print); cout << end1;`

the loop is very efficient that the view will only be executed when `for_each` is run.

## Range Algorithms

### Projection Parameters

Projections provide first class filtering predicate independent of function to be invoked. For example, if we want to sort a vector of 2-D objects by its first index, we can write an auto of return first index in the third entry without defining the compare operator.

### Range adaptor equivalence

There are 3 equivalent forms:
- adaptor ( ranges, args ...)
- adaptor (arg...) (range)
- range | adaptor (args...)

and their equivalence can be checked either by contents or `ranges::equal`. 

Unher the hood, each range/adaptor adds a wrappers of view class out of objects. The stack decides how to filter the iterators and the equivalence of the range types.

## String View & Span

String_view and span are not technically part of ranges but they are designed to satisfy the concept.

## Performance

The compile time of using ranges is similar to using loops and iterators. 

The development of it is still at a beginning stage. The runtime meassurement is application limited. There are theories developed that believe runtime of it will be same as STL methods as the algorithm behind them are similar.

The `boost.range` have been being used for a decade and it does make the code cleanner.
