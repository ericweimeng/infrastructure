# Built-ins

## Functions

### all\(\)

Return `True` if all elements of the _iterable_ are true \(or if the iterable is empty\). Equivalent to:

```text
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```

#### Time Complexity

O\(size of iterable\)

### Zip\(\)

Make an iterator that aggregates elements from each of the iterables.

Returns an iterator of tuples, where the _i_-th tuple contains the _i_-th element from each of the argument sequences or iterables. The iterator stops when the shortest input iterable is exhausted. With a single iterable argument, it returns an iterator of 1-tuples. With no arguments, it returns an empty iterator. Equivalent to:

```text
def zip(*iterables):
    # zip('ABCD', 'xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]
    while iterators:
        result = []
        for it in iterators:
            elem = next(it, sentinel)
            if elem is sentinel:
                return
            result.append(elem)
        yield tuple(result)
```

The left-to-right evaluation order of the iterables is guaranteed. This makes possible an idiom for clustering a data series into n-length groups using `zip(*[iter(s)]*n)`. This repeats the _same_ iterator `n` times so that each output tuple has the result of `n` calls to the iterator. This has the effect of dividing the input into n-length chunks.

[`zip()`](https://docs.python.org/3/library/functions.html#zip) should only be used with unequal length inputs when you don’t care about trailing, unmatched values from the longer iterables. If those values are important, use [`itertools.zip_longest()`](https://docs.python.org/3/library/itertools.html#itertools.zip_longest) instead.

[`zip()`](https://docs.python.org/3/library/functions.html#zip) in conjunction with the `*` operator can be used to unzip a list:&gt;&gt;&gt;

```text
>>> x = [1, 2, 3]
>>> y = [4, 5, 6]
>>> zipped = zip(x, y)
>>> list(zipped)
[(1, 4), (2, 5), (3, 6)]
>>> x2, y2 = zip(*zip(x, y))
>>> x == list(x2) and y == list(y2)
True
```

