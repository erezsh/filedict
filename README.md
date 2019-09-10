FileDict is a dictionary interface that saves and loads its data from an sqlite database using keys.
FileDict dictionary provides ACID access, has virtually no size limit, and can be accessed by several processes concurrently.

It is meant as a quick-and-simple general-purpose solution. It is rarely the best solution, but it is usually good enough, and much faster to set-up.

Performance obviously cannot compare to the builtin dictionary, but it is reasonable and of low complexity (refer to sqlite for more details on that).

Examples
-----------
```python
    >>> from filedict import FileDict
    >>> d=FileDict("example.dict")
    >>> d['bla'] = 10
    >>> d[(2,1)] = ['hello', (1,2) ]
    #-- exit & open a new session --
    >>> import filedict
    >>> d=FileDict("example.dict")
    >>> print d['bla']
    10
    >>> print d.items()
    [['bla', 10], [(2, 1), ['hello', (1, 2)]]]
    >>> print dict(d)
    {'bla': 10, (2, 1): ['hello', (1, 2)]}
```

Using batch mode for fast writing:

```python
    >>> d=FileDict("try.dict")
    >>> with d.batch:  # using .batch suspend commits, making a batch of changes quicker
    >>>    for i in range(100000):
    >>>            d[i] = i**2
    # This runs for a few seconds
    >>> print len(d)
    100000
    >>> del d[103]
    >>> print len(d)
    99999
```

Limitations
-----------

All data (keys and values) must be pickle-able
All keys must be hashable (perhaps this could be removed in the future, by hashing the pickled key)
Both keys and values are stored as a copy, so changing them after assignment will not update the dictionary.


Future
-------

Additions in the future may include:

* An LRU-cache for fetching entries
* Support for more databases (i.e. other than Sqlite)

