---
layout: post
title: Flattening a list in Python
comments: true
---

Recently I saw a question to `flatten a list` and I thought of solving it in Python. Unfortunately there isn't any built-in flatten function in Python. So I will discuss some different ways to solve it.

# Recursive way
This is actually simple. We just iterate through the list and for each item, if it's a list then we call the function again recursively. Otherwise we just add the element to the result array.

```Python
def flatten(data):
    """Recursive way to flatten a list."""
    if data is None or len(data) == 0:
      return data
    else:
      result = []
      for item in data:
        if isinstance(item, list):
          yield result.extend(flatten(item))
        elif item != '':
          result.append(item)
      return result
```

Alternatively we can use the `__yield__` keyword to generate the element.

# Using __morph__ package
There's a package called __morph__. It contains a flatten function which we can use to solve the problem. But let's install the package first.

```bash
$ pip install morph
```
Now let's write our flatten function.

```Python
import morph

def flatten_using_morph(data):
    """Flatten a list using morph package."""
    return morph.flatten(data)
```

# __funcy__ package
Similar to __morph__, there's another package called __funcy__. It contains a function called flatten which we can use to solve the problem.

```bash
$ pip install funcy
```
Now let's write our flatten function.

```Python
import funcy

def flatten_using_funcy(data):
    """Flatten a list using funcy package."""
    return funcy.flatten(data)
```

# __compiler.ast__ package
In Python 2 we have `compiler` package. There's also an exactly similar flatten function to solve this task.

```Python
import compile.ast

def flatten_using_compile(data):
    """Flatten using compile."""
    return flatten(data)
```

# Unit tests
Now let's write unit tests to test our functions. I used `nose` for unit testing.

```Python
from nose.tools import assert_equal

class TestFlatten(object):
    def test_flatten_list(self, solution):
        assert_equal(solution([[1, 2, [3]], [4]]), [1, 2, 3, 4])
        assert_equal(solution([1, [], 2, [3]]), [1, 2, 3])
        assert_equal(solution([x for x in xrange(4)]), [0, 1, 2, 3])
        assert_equal(solution([[1], 2, [[3], 4]]), [1, 2, 3, 4])
        assert_equal(solution(["abc",["def"],[]]), ['abc', 'def'])
        print 'Success: test_flatten'

def main():
    test = TestFlatten()
    test.test_flatten_list(flatten)
    test.test_flatten_list(flatten_using_funcy)
    test.test_flatten_list(flatten_using_morph)
    test.test_flatten_list(flatten_using_compile)

if __name__ == '__main__':
    main()
```

That's it. If you guys have any other ideas to solve the problem, let me know in the comments.
