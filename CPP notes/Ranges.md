# Ranges

## Range Basics

The simplest version of *range* is a pair of iterator experssion: {rbegin, rend} that include a range of iterator to do operation with. However, the iterators can be in different types as long as they can compare to each other. 

*Views* are ranges that a re usually defined on another range and transform the underlying range via some range algorithm or operation. In other words, they are "lazy version" of ranges.

There are many commonly used tools in the standard library sit in the definition of range. Collections like array, vector, map etc. are ranges. In OS, the directory_iterator and stream iterators are ranges. However, the container adapters like queue and stack are not ranges as they cant be iterated over. 

