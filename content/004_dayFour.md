Title: Day Four
Date: 2018-03-27 13:22
Modified: 2018-03-27 22:12
Category: Algorithms
Tags: python, programming, howto, algorithms, review, lists, tuples, arrays, strings, bytes, bytearrays
Slug: day-four
Authors: Christopher Tichenor
Summary: Reviewing arrays

## Reviewing Arrays

Follow-up from yesterday: I got the Blue and Green channels mixed up, as apparent from the orange-ish hue of the red rather than magenta-ish.

Now to today's lesson. 

---

There's 3 types of arrays that can hold different types of data in Python and 3 more that can hold only single types of data.

*   Different Types
    -   Lists
    -   Tuples
    -   `array.array`s
*   Single Types
    -   Strings
    -   Bytes
    -   Bytearrays

### Lists

#### Example:

```python
wordlist = ['this', 'is', 'some', 'text']
wordlist
# Returns: ['this', 'is', 'some', 'text']
wordlist[2] = 'changed'
wordlist
# Returns: ['this', 'is', 'changed', 'text']
```

#### Notes:

*   Not the most space-efficient.
*   Not the fastest collection type.
*   Mutable
*   [Big O for major operations](https://wiki.python.org/moin/TimeComplexity)
    -   Copy: O(n)
    -   Append: O(1)
    -   Insert: O(n)
    -   Delete Item: O(n)
    -   x in s: O(n)
    -   min, max: O(n)
    -   len: O(1)
*   Use cases
    -   Functional programming
    -   Holding different types of data which must be changed over time
    -   Non-performance critical
    -   Limited datasets
*   Other notes
    -   `del a[4:10]` can be used to delete multiple items from a list

### Tuples

#### Example:

```python
wordtuple = ('this', 'is', 'some', 'text')
wordtuple
# Returns: ('this', 'is', 'some', 'text')
wordtuple[2] = 'changed'
# Exception:
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: 'tuple' object does not support item assignment
```

#### Notes:

*   Also not super space-efficient.
*   Faster than lists
*   Not mutable.
*   Tuples are bound by the same limits as a list (where applicable, you won't append/insert/delete from a tuple).
*   Use cases
    -   Functional programming
    -   Holding different types of data in a read-only fashion
    -   More performance critical
    -   Larger datasets
*   Other notes
    -   A variant, the [namedtuple](https://pymotw.com/2/collections/namedtuple.html) is a good alternative to dictionaries, assuming the value won't be changed.

### `array.array`

#### Python 2 Example:

```python
# Behavior in Python 2
import array

wordarray = array.array('b', 'this is some text')
wordarray
# Returns: array('b', [116, 104, 105, 115, 32, 105, 115, 32, 115, 111, 109, 101, 32, 116, 101, 120, 116])
wordarray.tobytes()
# AttributeError:
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# AttributeError: 'array.array' object has no attribute 'tobytes'
dir(wordarray)
['__add__', '__class__', '__contains__', '__copy__', '__deepcopy__', '__delattr__', '__delitem__', '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmul__', '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'buffer_info', 'byteswap', 'count', 'extend', 'fromfile', 'fromlist', 'fromstring', 'fromunicode', 'index', 'insert', 'itemsize', 'pop', 'read', 'remove', 'reverse', 'tofile', 'tolist', 'tostring', 'tounicode', 'typecode', 'write']
wordarray.tounicode()
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# ValueError: tounicode() may only be called on type 'u' arrays
wordarray.tostring()
'this is some text'
```

#### Python 3 Example:

```python
from array import array

wordarray = array('b', 'this is some text')
# TypeError:
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: cannot use a str to initialize an array with typecode 'b'
wordarray = array('u', 'this is some text')
wordarray
# Returns: array('u', 'this is some text')
wordarray.tobytes()
# Returns: 
# b't\x00\x00\x00h\x00\x00\x00i\x00\x00\x00s\x00\x00\x00 \x00\x00\x00i\x00\x00\x00s\x00\x00\x00 \x00\x00\x00s\x00\x00\x00o\x00\x00\x00m\x00\x00\x00e\x00\x00\x00 \x00\x00\x00t\x00\x00\x00e\x00\x00\x00x\x00\x00\x00t\x00\x00\x00'
wordarray.tostring()
# Returns: 
# b't\x00\x00\x00h\x00\x00\x00i\x00\x00\x00s\x00\x00\x00 \x00\x00\x00i\x00\x00\x00s\x00\x00\x00 \x00\x00\x00s\x00\x00\x00o\x00\x00\x00m\x00\x00\x00e\x00\x00\x00 \x00\x00\x00t\x00\x00\x00e\x00\x00\x00x\x00\x00\x00t\x00\x00\x00'
```

`tobytes()` and `tostring()` are the same in Python 3, I assume the latter is going to disappear eventually. I suppose that makes sense as a byte representation for Unicode.

#### Notes:

*   More space efficient than lists or tuples
*   Not quite as fast as lists (usually)
*   Mutable
*   Other notes
    -   Basically a thin-wrapper around C arrays.
    -   Can [_maybe_](https://www.python.org/doc/essays/list2str/) be faster than lists

### Strings

#### Example:

```python
teststring = 'This is some text'
teststring
# Returns: 'This is some text'
```

#### Notes:

*   Good for storage of Unicode text
*   Immutable
*   Recursive: each character in a String is, in turn, another String

### Bytes

Continuing from the above example:

```python
testbytes = teststring.encode()
testbytes
# Returns: b'This is some text'
testbytes[0]
# Returns: 84
```

#### Notes:

*   Stores bytes (values between 0-255)
*   Each single byte is represented as an integer
*   In a sequence, bytes are represented as hexadecimal
*   Immutable
*   Good for storing things like 8bpc image data, hexadecimal data or ASCII characters

### Bytearrays

Further continuing from the above examples:

```python
testbytearray = bytearray(testbytes)
testbytearray
# Returns: bytearray(b'This is some text')
del(testbytearray[14])
testbytearray
# Returns: bytearray(b'This is some txt')
```

#### Notes:

*   Also stores bytes
*   Mutable, supports all of the operations of other [mutable sequence types](https://docs.python.org/3.1/library/stdtypes.html#typesseq-mutable)
*   Good for storing the same types of data, but in mutable form.