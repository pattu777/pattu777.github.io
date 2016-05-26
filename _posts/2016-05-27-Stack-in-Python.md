---
layout: post
title: Implementing a Stack in Python.
comments: true
---

As a continuation of some of my previous posts regarding data structures, in this post I will try to implement a `Stack` in Python.

## Stack:

> In computer science, a stack is an abstract data type that serves as a collection of elements, with two principal operations: push, which adds an element to the collection, and pop, which removes the most recently added element that was not yet removed. The order in which elements come off a stack gives rise to its alternative name, LIFO (for last in, first out). [^1]

Basically there are two ways to represent a stack.

* As an arrays.
* As a singly linked list.

Here I will try to implement an array based stack. Now let's define a stack class.

```python
class Stack(object):
    def __init__(self, size):
        self.size = size
        self.arr = []
        self.current_length = 0

    def push(self, item):
        # Code below

    def pop(self):
        # Code below

    def peek(self):
        # Code below

    def length(self):
        # Code below

    def is_empty(self):
        # Code below
```

The code is pretty fairly straight forward. The `__init__` function is the constructor of `Stack` class. __size__ represents the maximum number of elements which the stack can contain. __current_length__ holds the number of elements present in the stack. And the array __arr__ holds the actual elements of the stack.

Now let's implement the basic operations one by one.

### Inserting an element onto the stack
As the stack follows LIFO principle, we push all incoming elements to the end of the array.

```python
def push(self, item):
    if self.current_length == self.size:
        print "Stack is full."
    else:
        self.arr.append(item)
        self.current_length += 1
```

### Removing an element from the stack
During removal, the last element is popped from the array.

```python
def pop(self):
    if self.current_length == 0:
        print "Stack is already empty."
    else:
        self.arr.pop()
        self.current_length -= 1
```

### Fetch the top element

```python
def peek(self):
    if self.current_length == 0:
        print "Stack is empty."
    else:
        return self.arr[self.current_length-1]
```

### Get the number of elements

```python
def length(self):
    return self.current_length
```

### Check whether the stack is empty or not

```python
def is_empty(self):
    return self.current_length == 0
```

That's it.

* The average time of insertion is `O(1)`.
* The average time of removal is `O(1)`.
* Average time for search is `O(n)`.

The entire source code is available on my Github repository.[^2]

[^1]: [https://en.wikipedia.org/wiki/Stack_(abstract_data_type)](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))
[^2]: [https://github.com/pattu777/Algorithms-and-Data-structures](https://github.com/pattu777/Algorithms-and-Data-structures)
