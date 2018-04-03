Title: Day Five
Date: 2018-03-28 13:01
Modified: 2018-04-03 16:27
Category: Algorithms
Tags: python, programming, howto, algorithms, linkedlists
Slug: day-five
Authors: Christopher Tichenor
Summary: Linked Lists

## Linked Lists

To get this off to a banging start, here's a [fun article](https://www.codeproject.com/articles/340797/number-crunching-why-you-should-never-ever-ever-us). It's click-baity but conveys the idea that one should never use a linked list for number crunching. Upon reflection, that seems fairly obvious.

Today I'm using [this article](http://www.stackabuse.com/python-linked-lists/) for reference. There are two types of linked lists:

1. Single-linked
2. Double-linked

### Single-linked list

I understand the concept of the linked list, so I only really referred to the reference article for method names (and even then, I threw some of those out). I could post the piece-by-piece framing of the code, but I'll only post the final iteration of the code here:

```python
class LinkedList():
    def __init__(self, data):
        self.head = None
        self.tail = None
        self.length = 0
        self.add_item(data)
        
    def __repr__(self):
        current_node = self.head
        repr_out = 'Linked List: '
        while current_node is not None:
            if current_node is not self.head:
                repr_out = repr_out + ' -> ' + str(current_node.data)
            else:
                repr_out = repr_out + str(current_node.data)
            current_node = current_node.next
        return repr_out
    
    def add_item(self, data):
        item_node = ListNode(data)
        if self.length == 0:
            self.head = item_node
            self.tail = item_node
        else:
            self.tail.next = item_node
            self.tail = item_node
        self.length += 1
        
    def search(self, query):
        current_node = self.head
        previous_node = None
        while current_node is not None:
            if current_node.data == query:
                return (previous_node, current_node)
            else:
                previous_node = current_node
                current_node = current_node.next
        return
    
    def remove_item(self, query):
        previous_node, current_node = self.search(query)
        if current_node is not None:
            previous_node.next = current_node.next
        self.length -= 1
        return 'Deleted "{}"'.format(current_node.data)
        
class ListNode():
    def __init__(self, data):
        self.data = data
        self.next = None
        
    def __repr__(self):
        return str(self.data)
```

This seems to work pretty well in the informal tests I've made:

```python
test = LinkedList('blah')

test.head.data
# Returns: 'blah'

test.add_item('hey')

test.head.data
# Returns: 'blah'
```

So far so good.

```python
test.tail.data
# Returns: 'hey'

test.tail.next
```

That call returns nothing, as I'd expect being that it should return `None`.

So is the node after head the same as the tail?

```python
test.head.next.data
# Returns: 'hey'

test.head.next is test.tail
# Returns: True
```

It sure is!

```python
test.length
# Returns: 2

test.add_item('yo')

test.head.next.next.data
# Returns: 'yo'

test.head.next.data
# Returns: 'hey'
```

Now lets's see how `__repr__` works:

```python
test
# Returns: Linked List: blah -> hey -> yo
```

Nice. Can we delete items?

```python
test.remove_item('hey')

test.head.next.data
# Returns: 'yo'

test.head
# Returns: blah

test.tail
# Returns: yo
```

Looks like it's gone, let's check `__repr__`

```python
test
# Returns: Linked List: blah -> yo
```

How about search? (This was implicitly checked with `remove_item`, but still, nice to see.)

```python
test.search('blah')
# Returns: (None, blah)

test.search('yo')
# Returns: (blah, yo)
```

Okay, only a single-linked list for now, we'll do double-linked (hopefully) tomorrow.