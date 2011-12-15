FileDict is a dictionary interface that saves and loads its data from an sqlite database using keys.
FileDict dictionary provides ACID access, has virtually no size limit, and can be accessed by several processes concurrently.

It is meant as a quick-and-simple general-purpose solution. It is rarely the best solution, but it is usually good enough, and much faster to set-up.

Performance obviously cannot compare to the builtin dictionary, but it is reasonable and of low complexity (refer to sqlite for more details on that).

Examples
-----------
    $ python
    >>> import filedict
    >>> d=filedict.FileDict(filename="example.dict")
    >>> d['bla'] = 10
    >>> d[(2,1)] = ['hello', (1,2) ]
    -- exit --
    $ python
    >>> import filedict
    >>> d=filedict.FileDict(filename="example.dict")
    >>> print d['bla']
    10
    >>> print d.items()
    [['bla', 10], [(2, 1), ['hello', (1, 2)]]]
    >>> print dict(d)
    {'bla': 10, (2, 1): ['hello', (1, 2)]}

Using batch mode for fast writing:

    >>> d=filedict.FileDict(filename="try.dict")
    >>> with d.batch:  # using .batch suspend commits, making a batch of changes quicker
    >>>    for i in range(100000):
    >>>            d[i] = i**2
    (takes about 8 seconds on my comp)
    >>> print len(d)
    100000
    >>> del d[103]
    >>> print len(d)
    99999


Limitations
-----------

All data (keys and values) must be pickle-able
Keys must be hashable (perhaps this should be removed by hashing the pickled key)
Keys and values are stored as a copy, so changing them after assignment will not update the dictionary.


Future
-------

Additions in the future may include:

* An LRU-cache for fetching entries
* A storage strategy different than Sqlite

