Title: Day One
Date: 2018-03-22 18:41
Modified: 2018-03-27 13:16
Category: Algorithms
Tags: python, programming, howto, algorithms, arrays, datastructures
Slug: day-one
Authors: Christopher Tichenor
Summary: My first day of algorithms lessons: arrays

## Types of Arrays in Python

As noted in the Reddit article from yesterday, before going into algorithms you need to be solid on data structures and attempt to implement your own.

Initially I was going to attempt to do arrays, linked lists, and stacks/queues today, but there's enough to do in arrays themselves to fill at least a day's work.

## Arrays

I'm using [this article](https://dbader.org/blog/python-arrays) for guidance.

Arrays as defined here and in the article are collections of individual pieces of data that are stored contiguously in memory as opposed to pieces of data that are linked together and capable of occupying different areas of memory.

From this article, Python has 3 types of arrays for different types of data:

*   **Lists:** basically dynamic arrays that allocate and release data as needed based on the number of items in them and are mutable. They support mixed data types, but are space inefficient because of this.
*   **Tuples:** arrays that cannot be added to or removed from, but also support different data types. If you add elements to a tuple, you create a new tuple.
*   **array.array(s):** `array.array` must be imported from a Python module and can only hold one type of data. They are more space-efficient than the above two array types, and can be appended to.

There's another three types of arrays for single data types:

*   **Strings:** Immutable and only store single character `String` objects recursively.
*   **Bytes:** Also immutable, and can only store ints between 0-255
    ```python
    >>> bytes((0, 255))
    b'\x00\xff'
    ```
*   **Bytearrays:** Mutable form of a byte object, can be added to, modified, and deleted from. Converting to a `byte` object is an O(n) process.

[Here's](https://en.wikipedia.org/wiki/Array_data_type) the Wikipedia article on arrays. And [here's](https://stackoverflow.com/questions/4062752/in-what-structure-is-a-python-object-stored-in-memory#4062900) a StackOverflow description of low-level implementation of Python array types.

The task here to implement my own array in Python seems to be to create a Python object with the following attributes:

1.  Multiple items are stored in this object.
2.  Data is stored contiguously in memory.
3.  Data can be accessed by index.

1 and 3 seem to be fairly simple. 2, however, is not from what I can tell. Looking through the Python docs, it looks like the way to create new data types is via [PyObject](https://docs.python.org/3.6/extending/newtypes.html). Backing out to the [primary article that contains that article](https://docs.python.org/3.6/extending/index.html), I'll basically need to drop back to C to be able to store memory contiguously...

...Except even C doesn't seem to allow one to make an array without using an array, which feels like cheating. Perhaps a variation of [this](https://www.quora.com/How-do-I-create-a-matrix-of-n*n-without-using-an-array-in-C-programming-language) might work (wait, nevermind, it also uses an array). I suppose I could also drop back to [assembler](http://www.cs.uwm.edu/classes/cs315/Bacon/Lecture/HTML/ch12s04.html) but _wow_ is that going into the weeds.

Not a great start. Today's lesson is a bit of a failure, but at least I learned a lot. Maybe the correct approach for this will become more obvious as I study more.