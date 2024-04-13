+++
title = '[C++] Some programming language tips and tricks'
date = 2024-02-17T17:09:01+08:00
draft = false
+++

原文链接：

[[C++] Some programming language tips and tricks - Codeforces](https://codeforces.com/blog/entry/126011)

Hello, Codeforces!

Here are some language tips and tricks I have discovered that could (hopefully) help you write more concise or efficient code. Note that all of the below tips are specific to the C++ programming language and the STL library.

# Creating a std::set or std::map from sorted data in 𝑂(𝑛) time

The sorted containers `set` and `map` are usually implemented as red–black trees, meaning that their `insert` operation runs in 𝑂(log𝑛) time by default. Naturally, repeating the `insert` operation 𝑛 times would result in a time complexity of 𝑂(𝑛log𝑛).

```
vector<int> a = /* ... */;
set<int> s1;
for (int e : a) { s1.insert(e); } // O(n log n)
```

However, `set` and `map` also provide a constructor method, that directly constructs the data structure from a given pair of iterators. In most implementations of C++, if the data contained within the range is already sorted, the constructor instead runs in 𝑂(𝑛) time.

```
vector<int> a = /* some sorted data */;
set<int> s2(a.begin(), a.end()); // O(n)
int arr[n + 10] = /* more sorted data */;
set<int> s3(&arr[1], &arr[n] + 1); // ditto
```

Here is a simple benchmark code that I have written to illustrate the above performance difference. The code generates 𝑛 integers in the range [0,𝑛], sorts them, then inserts them to sets using the two above methods. It can be seen, that the 𝑂(𝑛) method generally runs in half the time of the 𝑂(𝑛log𝑛) method:

**Benchmark code**

[bm.cpp](%5BC++%5D%20Some%20programming%20language%20tips%20and%20tricks%201e4c7cb0026844f4aa308d1b432fdae4/bm.cpp)

**Results (nanoseconds)**

If directly constructing the set (or map) is not possible (e.g. due to prior operations needing to be done on the set, or due to the data being online, etc.), the C++ `set` container provides an overload of the `.insert()` method that allows you to also specify a 'hint' iterator, which points to the element just greater than the element to be inserted. In other words, if `e` is to be inserted into `s`, this 'hint' iterator should be equal to `s.lower_bound(e)`.

If the given hint iterator happens to be correct, then the `.insert()` call runs in amortised 𝑂(1) time. Otherwise, the call still runs in `O(\log n)` time but may be slower than the regular `.insert()` call.

```
s.insert(s.lower_bound(10), 10);// The 'insert' operation itself runs in amortised O(1) time
```

The above example is not very helpful, since the `.lower_bound()` call itself already takes 𝑂(log𝑛) time, defeating the entire purpose of the optimisation. However, this can be useful if we already have extra information about the position of the to-be-added element. In particular,

- If the element `e` is greater than all elements currently in `s`, we can write `s.insert(s.end(), e)`.
- Similarly, if the element `e` is less than all elements currently in `s`, we can write `s.insert(s.begin(), e)`.

Similar operations can also be done on the `map` data structure, except that the second argument (element to be inserted) now needs to be a key-value `std::pair`.

# Reducing the number of memory allocations using .reserve()

In the vast majority of C++ STL implementations, the containers `vector` and `string` have an internal buffer that is used to store data. Whenever a new element needs to be inserted while this buffer is full, the data structure first allocates a new, larger buffer, copies all data over to it, and then deallocates the original buffer. Whenever this occurs with a `.push_back()` call, the time taken for that call would be 𝑂(𝑛). Since such memory reallocations can be very expensive, it would be ideal if we could decrease the number of reallocations.

The `.reserve()` method, available for both `vector`s and `string`s, can lessen the frequency of reallocations by making the data structure expand its buffer to a provided size. For instance, if `v` is a vector, `v.reserve(10);` expands `v`'s internal buffer to have a size of at least 10.

`.reserve()` has the following properties:

- Iterators and references are invalidated *if and only if* the provided size exceeds the current size of the buffer.
- If the buffer is already big enough, nothing happens. `.reserve()` cannot be used to decrease the size of a container.
- If the `.reserve()` call did change the size of the buffer, the time taken would be 𝑂(𝑛), where 𝑛 is the size of the buffer.
- `.reserve()` only changes the *internal* size of the container, not the *logical* size. For instance, the following code segment is still illegal (undefined behaviour):

```
vector<int> v(5, 1);// v has size 5
v.reserve(100);// v now has buffer size at least 100, but logically still size 5
v[10] = 20;// undefined behaviour
```

It is important to note that, since `.reserve()` calls themselves cause reallocations to occur, they should be used sparingly, preferably only once just after the declaration of the container itself. For instance, the following code is highly inefficient and, theoretically, would increases the time taken from 𝑂(𝑛) to 𝑂(𝑛2):

```
int n;
cin >> n;
vector<int> a;
for (int i = 0; i < n; i++) {
    a.reserve(i + 1);
int e;
    cin >> e;
    a.push_back(e);
}
```

**Correct version ($O(n)$)**

Additionally, for `string`s, directly inputting it from `std::cin` would also cause it to expand its buffer size dynamically. On Codeforces, many problems would give you the size of the string first before the string itself. In these scenarios, the following code can be written to read the string more effectively:

```
int n;
string s;
cin >> n;
s.reserve(n);
cin >> s;
```

Finally, note that calling `.reserve()` with an argument exceeding the actual size of the container can cause memory to be wasted. If memory is critical but the exact size of the container is not known, we can use the `.shrink_to_fit()` method to reduce the buffer size to what is strictly necessary to carry the data:

```
string s;
s.reserve(100);
cin >> s;
// s is likely to be less than 100 characters, therefore shrink to save memory
s.shrink_to_fit();
```

Note that, unlike `.reserve()`, `.shrink_to_fit()` is **not** guaranteed to have any effect on the container, though for most implementations, it does reduce the memory usage.

# The macro ONLINE_JUDGE

When a piece of C++ code is run on Codeforces, the platform automatically defines a macro `ONLINE_JUDGE`. This can be useful in a variety of scenarios when you want to differentiate whether the code is being tested on your device or being judged online.

For instance, a common optimisation for improving I/O efficiency is to untie the standard streams `cin` and `cout` using the following line of code: `cin.tie(0)`. By default, C++ flushes the `cout` stream whenever an input operation is performed, so that the user can see the output before being requested to input. While this would slow down the program and increase the running time, it could still be a useful feature to have when testing the code. Therefore, we can conditionally enable this optimisation using the following code segment:

```
#ifdef ONLINE_JUDGE
    cin.tie(nullptr);
#endif
```

This way, the output is not affected when the code is being run on your local device. Be careful not to misspell `ONLINE_JUDGE`! Otherwise the optimisation will not be applied anywhere and may cause great performance penalties.

Another common usage of `ONLINE_JUDGE` is writing debug output functions. You can write a debugging function that only outputs to `cout` when not officially submitted and judged using the following code template:

```
void debug(/* any parameters */) {
#ifndef ONLINE_JUDGE
// code that produces debug output#endif
}
```

The `debug` function can now be used anywhere inside your code to produce output only when testing. When judging, the function is a no-op. Be careful not to mix up the preprocessor directives — here it is `ifndef` instead of `ifdef`, which causes the code that follows to be compiled only when the macro is **not** defined.

# Explicit types for min and max

By default, the functions `min` and `max` from `<algorithm>` cause compile errors when the arguments are not of the same type. For instance, the below statements all cause compilation failures:

```
min(3, 2.5);
max(4, 7LL);
min('a','d' + 1);
max(10, a.size());// a is a container
```

In these cases, you can explicitly specify a type for both parameters to be converted to in order to compile. Beware of losses to range/precision, however.

```
min<double>(3, 2.5);
max<longlong>(4, 7LL);
min<char>('a','d' + 1);// narrows the second argument, which is an int
max<int>(10, a.size());// technically narrows a.size() but usually won't matter
```

# Lambda comparator for priority_queue

You can create a `priority_queue` whose comparator function is a lambda without having to create a separate class. For instance, to create a `priority_queue` for Dijkstra's algorithm where entries are stored as node–distance pairs, one can write:

```
using ll =longlong;
usingentry_t = pair<int, ll>;

auto cmp = [](entry_t a,entry_t b) {return a.second > b.second; };
priority_queue<entry_t, vector<entry_t>,decltype(cmp)> q(cmp);
```

In my opinion, this is more natural than using (negative distance)–node pairs, for which the default sorting would have worked, but I find it more difficult to work with and possibly more bug-prone.

# Miscellaneous helpful STL functions

The following are some lesser-known STL functions that I have found useful in a variety of scenarios. I find that they can make code more concise by a considerable degree.

- `div` and `lldiv` (`<cstdlib>`): takes two integer arguments (int for div, long long for lldiv), returns a structure that contains the quotient and the remainder but only takes one operation. Can be unpacked using structured bindings: auto [quot, rem] = div(a, b);
- `reduce` (`<numeric>`): given a range (an iterator pair), returns the sum of all elements in it. Beware of arithmetic overflows, however.
- `minmax_element` (`<algorithm>`): given a range, returns a pair of iterators (pointers for arrays) to the minimum and maximum element inside that range. This is usually better than two separate calls to min_element and max_element since the range is traversed only once. Can be unpacked using structured bindings, too.
- `inclusive_scan` and `exclusive_scan` (`<numeric>`): generates the prefix sum for a range. The nth output of inclusive_scan includes the nth element in the sum, while that of exclusive_scan does not.
- `swap` (`<utility>`): swaps two elements. Though we can implement swapping ourselves in merely 3 lines, the frequency of this operation makes this function still useful. Additionally, swaps for container classes such as vectors or strings are usually more optimised than swapping via a temporary variable since most implementations of STL allow swaps to access the innards of these structures.

# Conclusion

So, the above is some C++ tips that I would like to share with you all. If you have any feedback regarding the content or the format, please kindly leave them in the comments below. Thank you very much for reading this far!