# Ranges

## Range Basics

The simplest version of *range* is a pair of iterator experssion: {rbegin, rend} that include a range of iterator to do operation with. However, the iterators can be in different types as long as they can compare to each other. 

There are many commonly used tools in the standard library sit in the definition of range. Collections like array, vector, map etc. are ranges. In OS, the directory_iterator and stream iterators are ranges. However, the container adapters like queue and stack are not ranges as they cant be iterated over. 

*Views* are ranges that a re usually defined on another range and transform the underlying range via some range algorithm or operation. In other words, they are "lazy version" of ranges. The important points here are views doesn't own the elements and all the methods are in *O(1)*. That means, views can also be traeted as methods, which makes most containers not views as copying the containers is almost never in constant time.

*Range adaptors* make ranges into views. It create pipelines of lizily evaluated transfomations: `auto filtered_range = range | std::ranges::views::filter( some_filter);`.

The views and range algorithm can be combined together to make codes cleaner. For example, if we have 

`auto print = [] (int i) { cout << i << " ";};
auto is_even = [] (int i) { return i % 2 == 0;};
vector <int> v { 6, 2, 3, 4, 5, 6};

auto after_leading_evens = rng::views::drop_while(v, is_even);
rng::for_each(after_leading_evens, print);
cout << end1;`
d
