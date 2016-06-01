---
layout: post
title: Binary search tree in Python with simple unit tests.
comments: true
---
Last week I was reading an article about testing in Python.[^1] It explained about various testing frameworks available in Python(`unittest`, `py.test`, `nose` etc.) with some examples. It was a really interesting article. So In this post I will try to implement a binary search tree and write some simple unit tests using the `unittest` library in Python.

### Binary Search Tree
> A binary search tree (BST) is a binary tree where each node has a Comparable key (and an associated value) and satisfies the restriction that the key in any node is larger than the keys in all nodes in that node's left subtree and smaller than the keys in all nodes in that node's right subtree.[^2]

A node of a binary search tree is as shown below.

```bash
         data
       /      \
    left      right
```

```python
class Node(object):
    def __init__(self, data=None, left=None, right=None):
        self.data = data
        self.left = left
        self.right = right
```

For testing purpose , we will use the following binary search tree.

```bash
        100
      /     \
     20     500
   /    \
  10    30
           \
           40
```

Let's create a file `bst.py` to implement a binary search tree class.

```python
class BinarySearchTree(object):
    def __init__(self, root=None):
        self.root = root

    def get_root(self):
        return self.root

    def insert(self, item):
        # Code below

    def search(self, node, item):
        # Code below

    def size(self, node):
        # Code below

    def inorder(self, node):
        # Code below

    def preorder(self, node):
        # Code below

    def postorder(self, node):
        # Code below

    def get_min(self, node):
        # Code below

    def get_max(self, node):
        # Code below
```

The only thing to note is that I made the `root` of tree `None` in the constructor `__init__` function. Now it's time to implement common operation like insertion, searching, traversal etc.

#### Insert an element

```
Function insert(item):
======================

1. If root is None,
  * Create a new node with item as key
  * Make it root.

2. Else find a new position by comparing it with each node.
  * If item < key of current node, search in the left subtree.
  * If item > key of current node, search in the right subtree.
  * If the values are identical, ignore it.

3. Once the appropriate position is found,
  * Create a new node with item as key
  * Insert at that position.
```

```python
def insert(self, item):
    if self.root is None:
        self.root = Node(item)
    else:
        nd = self.root
        while nd is not None:
            if item < nd.data:
                if nd.left is None:
                    nd.left = Node(item)
                    return
                else:
                    nd = nd.left
            else:
                if nd.right is None:
                    nd.right = Node(item)
                    return
                else:
                    nd = nd.right
```

#### Search an element
```
Function search(item):
======================

1. If root is None, return False.
2. Else recursively search the entire tree.
  * If item < key of current node, search in the left subtree.
  * If item > key of current node, search in the right subtree.
  * If found, return True.
3. If not found till now, return False.
```

```python
def search(self, node, item):
   if node is None:
       return False
   else:
       if node.data == item:
           return True
       elif node.data < item:
           node = node.right
       else:
           node = node.left
```

#### Get the number of elements
```python
def size(self, node):
    if node is None:
        return 0
    else:
        return 1 + self.size(node.left) + self.size(node.right)
```

#### In-order traversal
```python
def inorder(self, node):
    if node:
        self.inorder(node.left)
        print node.data
        self.inorder(node.right)
```

#### Pre-order traversal
```python
def preorder(self, node):
    if node:
        print node.data
        self.preorder(node.left)
        self.preorder(node.right)  
```

#### Post-order traversal
```python
def postorder(self, node):
    if node:
        self.postorder(node.left)
        self.postorder(node.right)
        print node.data
```

#### Fetch the minimum element
```
Function get_minimum():
=======================

1. If root is None, return None.
2. Else recursively search in the left subtree.
  * If a match is found, return True.
3. If end of the tree is reached and search is unsuccessful return False.
```

```python
def min(self, node):
    if node.left is None:
        return node.data
    else:
        return self.get_min(node.left)
```

#### Fetch the maximum element
```
Function get_maximum():
=======================

1. If root is None, return None.
2. Else recursively search in the right subtree.
  * If a match is found, return True.
3. If end of the tree is reached and search is unsuccessful,
  * return False.
```

```python
def max(self, node):
    if self.root is None:
        return "Tree is empty."
    else:
        if node.right is None:
            return node.data
        else:
            return self.get_max(node.right)
```

Now let's create a new file `test_bst.py` and write some unit tests.

```python
#!/usr/bin/env python

import sys
import unittest

from bst import Node, BinarySearchTree

class BstTest(unittest.TestCase):
    def __init__(self, *args, **kwargs):
        super(BstTest, self).__init__(*args, **kwargs)
        self.tree = BinarySearchTree()
        self.arr = [100, 20, 500, 30, 10, 40]

        for x in self.arr:
            self.tree.iterative_insert(x)

    def test_search(self):
        self.assertTrue(self.tree.search(self.tree.get_root(), 30))
        self.assertFalse(self.tree.search(self.tree.get_root(), 12312))

    def test_size(self):
        self.assertEqual(self.tree.size(self.tree.get_root()), 6)

    def test_depth(self):
        self.assertEqual(self.tree.depth(self.tree.get_root()), 4)

    def test_min(self):
        self.assertEqual(self.tree.min(self.tree.get_root()), 10)

    def test_max(self):
        self.assertEqual(self.tree.max(self.tree.get_root()), 500)

    def test_display(self):
        print "Inorder Traversal: "
        self.tree.inorder(self.tree.get_root())
        print "Preorder Traversal: "
        self.tree.preorder(self.tree.get_root())
        print "Postorder Traversal: "
        self.tree.postorder(self.tree.get_root())


if __name__ == '__main__':
    unittest.main()
    sys.exit(0)
```

The code is very simple. The only thing is that I am not performing any tests in the `test_display` function. Now let's run our unit tests and hope all the tests pass.

```bash
$ python test_bst.py
Inorder Traversal:
10
20
30
40
100
500
Preorder Traversal:
100
20
10
30
40
500
Postorder Traversal:
10
40
30
20
500
100
......
----------------------------------------------------------------------
Ran 7 tests in 0.001s

OK
```
I haven't implemented how to remove a node from the bst. As it's a little bit complicated and the purpose of this post was to implement common operations with `unit tests`. After writing unit tests, I think how silly I was to test my code using `print statements` before.

* __The average time of insertion is `O(logn)` and `O(n)` in worst case.__
* __The average time of removal is `O(logn)` and `O(n)` in worst case.__
* __Average time for search is `O(logn)` and `O(n)` in worst case.__

This code along with all other data structure implementations is available on my [Github repository](https://github.com/pattu777/Algorithms-and-Data-structures). That's it. 

[^1]: [http://docs.python-guide.org/en/latest/writing/tests/](http://docs.python-guide.org/en/latest/writing/tests/)
[^2]: [https://en.wikipedia.org/wiki/Binary_search_tree](https://en.wikipedia.org/wiki/Binary_search_tree)
