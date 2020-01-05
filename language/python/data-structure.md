# Data Structure

### Unpacking

#### Unpacking a Sequence into Separate Variables

Any sequence \(or iterable\) can be unpacked into variables using a simple **assignment operation**. Unpacking actually works with any object that happens to be iterable, not just tuples or lists. This includes strings, files, iterators, and generators.

```bash
>>> data = [ 'ACME', 50, 91.1, (2012, 12, 21) ]
>>> name, shares, price, (year, mon, day) = data
>>> name
'ACME'
>>> year
2012
>>> mon 12
>>> day 21

>>> s = 'Hello'
>>> a, b, c, d, e = s 
>>> a
'H'
>>> b
'e'
>>> e
'o'
```

#### Unpacking Elements from Iterables of Arbitrary Length

You need to unpack N elements from an iterable, but the iterable may be longer than N elements, causing a “too many values to unpack” exception. Python **“star expressions”** can be used to address this problem.

Star unpacking can also be useful when combined with certain kinds of string processing operations, such as splitting. 

```bash
>>> record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212') 
>>> name, email, *phone_numbers = user_record
>>> name
'Dave'
>>> email
'dave@example.com'
>>> phone_numbers ['773-555-1212', '847-555-1212']

>>> line = 'nobody:*:-2:-2:Unprivileged User:/var/empty:/usr/bin/false' 
>>> uname, *fields, homedir, sh = line.split(':')
>>> uname
'nobody'
>>> homedir '/var/empty'
>>> sh '/usr/bin/false'
```

Sometimes you might want to unpack values and throw them away. You can’t just specify a bare \* when unpacking, but you could use a common throwaway variable name, such as \_ or ign \(ignored\). For example:

```bash
>>> record = ('ACME', 50, 123.45, (12, 18, 2012)) 
>>> name, *_, (*_, year) = record
>>> name
'ACME'
>>> year
2012
```

### Keeping the Last N Items

You want to keep a limited history of the last few items seen during iteration or during some other kind of processing.

Keeping a limited history is a perfect use for a **collections.deque**.

### Finding the Largest or Smallest N Items

You want to make a list of the largest or smallest N items in a collection.

The **heapq module**

This module provides an implementation of the heap queue algorithm, also known as the priority queue algorithm.

Heaps are binary trees for which every parent node has a value less than or equal to any of its children. This implementation uses arrays for which `heap[k] <= heap[2*k+1]` and `heap[k] <= heap[2*k+2]` for all _k_, counting elements from zero. For the sake of comparison, non-existing elements are considered to be infinite. The interesting property of a heap is that its smallest element is always the root, `heap[0]`.

