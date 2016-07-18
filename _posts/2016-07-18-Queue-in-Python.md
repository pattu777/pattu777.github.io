---
layout: post
title: Implementing a Queue in Python.
comments: true
---

# Queue:

> In computer science, a queue is a particular kind of abstract data type or collection in which the entities in the collection are kept in order and the principal (or only) operations on the collection are the addition of entities to the rear terminal position, known as enqueue, and removal of entities from the front terminal position, known as dequeue. [^1]

A queue is a First-In-First-Out (FIFO) data structure. It means the first element added to the queue will be the first one to be removed. Similar to a stack, a queue can be implemented in the following two ways.

* As an array.
* As a singly linked list.

Here I will try to implement an array based queue. Now let's define a queue class.

```python
class Queue(object):
    def __init__(self, max_size=0):
        self.size = max_size
        self.arr = []

    def enqueue(self, item):
        # Code below.

    def dequeue(self):
        # Code below.

    def is_empty(self):
        # Code below.

    def is_full(self):
        # Code below.

    def front(self):
        # Code below.

    def rear(self):
        # Code below.
```

The code is fairly straight forward. The `__init__` function is the constructor of `Queue` class. `size` represents the maximum number of elements which the queue can contain. And the array `arr` holds the actual elements of the queue.

Now let's implement the basic operations one by one.

# Inserting an element
As the queue follows FIFO principle, we push all incoming elements to the end of the array.

```python
def enqueue(self, item):
    if self.is_full():
        print "Queue is full."
    else:
        self.arr.append(item)
```

# Removing an element from the Queue
During removal, the first element is popped from the array.

```python
def dequeue(self):
    if self.is_empty():
        print "Queue is empty."
    else:
        self.arr.pop(0)
```

# Fetch the front element

```python
def front(self):
    if self.is_empty():
        print "Queue is empty."
    else:
        print self.arr[0]
```

# Fetch the rear element

```python
def rear(self):
    if self.is_empty():
        print "Queue is empty."
    else:
        print self.arr[len(self.arr)-1]
```

# Check whether the queue is empty or not

```python
def is_empty(self):
    return len(self.arr) == 0
```

# Check whether the queue is full or not

```python
def is_full(self):
    return len(self.arr) == self.size
```

That's it.

* The average time of insertion is `O(1)`.
* The average time of removal is `O(1)`.
* Average time to fetch the front and rear item is `O(1)`.

The entire source code is available on my Github repository.[^2]

[^1]: [https://en.wikipedia.org/wiki/Queue_(abstract_data_type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
[^2]: [https://github.com/pattu777/Algorithms-and-Data-structures](https://github.com/pattu777/Algorithms-and-Data-structures)
