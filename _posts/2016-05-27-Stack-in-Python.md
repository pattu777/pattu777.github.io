---
layout: post
title: Implementing a Stack in Python.
comments: true
---

In previous posts, I have implemented a binary search tree and a singly linked list. In this post I will try to implement a `Stack` in Python.

## Stack:

> In computer science, a stack is an abstract data type that serves as a collection of elements, with two principal operations: push, which adds an element to the collection, and pop, which removes the most recently added element that was not yet removed. The order in which elements come off a stack gives rise to its alternative name, LIFO (for last in, first out). [^1]

Basically there are two ways to represent a stack.

* As an array.
* As a singly linked list.

Here I will try to implement an array based stack. Now let's define a stack class.

```python
class Stack(object):
    def __init__(self, size):
        self.size = size
        self.arr = []

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

    def is_full(self):
        # Code below
```

The code is fairly straight forward. The `__init__` function is the constructor of `Stack` class. `size` represents the maximum number of elements which the stack can contain. And the array `arr` holds the actual elements of the stack.

Now let's implement the basic operations one by one.

### Inserting an element onto the stack
As the stack follows LIFO principle, we push all incoming elements to the end of the array.

```python
def push(self, item):
    if len(self.arr) == self.size:
        print "Stack is full."
    else:
        self.arr.append(item)
```

### Removing an element from the stack
During removal, the last element is popped from the array.

```python
def pop(self):
    if len(self.arr) == 0:
        print "Stack is already empty."
    else:
        self.arr.pop()
```

### Fetch the top element

```python
def peek(self):
    if len(self.arr) == 0:
        print "Stack is empty."
    else:
        return self.arr[len(self.arr)-1]
```

### Get the number of elements

```python
def length(self):
    return len(self.arr)
```

### Check whether the stack is empty or not

```python
def is_empty(self):
    return len(self.arr) == 0
```

### Check whether the stack is full or not

```python
def is_full(self):
    return len(self.arr) == self.size
```

That's it.

* The average time of insertion is `O(1)`.
* The average time of removal is `O(1)`.
* Average time for search is `O(n)`.

The entire source code is available on my Github repository.[^2]

[^1]: [https://en.wikipedia.org/wiki/Stack_(abstract_data_type)](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))
[^2]: [https://github.com/pattu777/Algorithms-and-Data-structures](https://github.com/pattu777/Algorithms-and-Data-structures)
