Title: Day Two
Date: 2018-03-24 18:30
Modified: 2018-03-27 13:15
Category: Algorithms
Tags: python, programming, howto, algorithms, arrays, datastructures
Slug: day-two
Authors: Christopher Tichenor
Summary: Creating arrays using, well, arrays.

## Using `array.array` and limiting its capabilities

Well, if you need to use an array to create an array, I guess that's what I need to do. I suppose this makes sense given that the original computers were giant, inefficient memory arrays, so arrays are built in to the system.

These exercises apply different limits on Python's "built-in" array.array construct.

By the way, yes, I realize I could just inherit the previous classes for each iteration of the `IntArray`, but I'm not confident enough in my inheritance knowledge just yet.

With that out of the way...

---

First, we're just going to make a prototype of our array that doesn't really have any limits, other than being limited to `int`s:

```python
from array import array

class IntArray:
    def __init__(self, coll):
        self.data = array('i', coll)
```

Now let's test it out:

```python        
test_array = IntArray((1,2,3,4,5))

print(test_array.data)
# Returns: array('i', [1, 2, 3, 4, 5])
```

So far so good, but what if we create a new `IntArray` with a String?

```python
test2 = IntArray('blah')
# Results in an exception, as expected.
```

---

Now, let's give our array the ability to add items. We'll need to define an `add_data` method and call it in `__init__`, leveraging the `extend` functionality of `array.array`:

```python
class IntArrayAdd:
    def __init__(self, coll):
        self.data = array('i', [])
        self.add_data(coll)
        
    def add_data(self, coll):
        self.data.extend(coll)
```

Now to test:

```python
test_array2 = IntArrayAdd((6,7,8,9,10))

print(test_array2.data)
# Returns: array('i', [6, 7, 8, 9, 10])
```

Let's add some data:

```python
test_array2.add_data([1,2,3,4,5])

print(test_array2.data)
# Returns: array('i', [6, 7, 8, 9, 10, 1, 2, 3, 4, 5])
```

---

Now let's add an explicit limit on the amount of items that the array can hold, and throw an `AttributeError` if we try to stuff too much into it:

```python
class IntArrayAddCheck:
    def __init__(self, coll, length):
        self.data = array('i', [])
        self.length = length
        self.add_data(coll)
        
    def add_data(self, coll):
        if (len(coll) + len(self.data)) > self.length:
            raise AttributeError('There are too many items for the specified limit of this array.')
        else:
            self.data.extend(coll)
```

Let's make sure we can't add too much to it when instantiating the class:

```python
# test_array3 = IntArrayAddCheck([1,2,3,4,5], 3)  
# Raises an AttributeError, as expected, we fed it 5 ints but only told it to hold 3.
```

So far so good. Let's be nice to it now:

```python
test_array4 = IntArrayAddCheck([1,2,3,4,5], 5)

print(test_array4.data)
# Returns: array('i', [1, 2, 3, 4, 5])
```

Now let's be mean again and try to give it more data:

```python
test_array4.add_data((4,5,6,7))
# Raises an AttributeError
```

w00t.

---

This kinda bugs me though:

```python
print(test_array4)
# Returns: <__main__.IntArrayAddCheck object at 0x106f20780>
```

That's not great, but I also don't want to return the array data as is, since we know that this is going to be an `int` array. I just want to return the values as they appear inside the array.

So, let's add a `__repr__` method to fix that:

```python
class IntArrayAddCheckRepr:
    def __init__(self, coll, length):
        self.data = array('i', [])
        self.length = length
        self.add_data(coll)
        
    def __repr__(self):
        return str(self.data.tolist())
        
    def add_data(self, coll):
        if (len(coll) + len(self.data)) > self.length:
            raise AttributeError('There are too many items for the specified limit of this array.')
        else:
            self.data.extend(coll)
```

And now to test that:

```python
test_array6 = IntArrayAddCheckRepr((10,9,8,7,6), 5)
print(test_array6)
# Returns: [10, 9, 8, 7, 6]
```

Yay.

Slight problem, though. We can do this:

```python
test_array6.data.extend((3,4,5))
print(test_array6)
# Returns: [10, 9, 8, 7, 6, 3, 4, 5]
```

I think I'll need to leverage inheritance to block this. Eh, out of time, have to wait 'til later.